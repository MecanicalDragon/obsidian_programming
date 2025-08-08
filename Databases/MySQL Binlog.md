**InnoDB** - engine for **MySQL**

**Redo log** - журнал восстановления InnoDB, аналог WAL в Postgres. Отвечает за восстановление бд после краша. В нем хранятся физические изменения страниц данных,
информация, необходимая для "переигрывания" завершённых транзакций после сбоя. При старте MySQL, если был крэш, — InnoDB "переигрывает" транзакции из redo log.

**Binlog** - источник правды для репликации и логгирования всех изменений, *Point-in-time recovery*. Мастерский бинлог читают реплики для соблюдения консистентности. Хранит данные в более высокоуровневом формате: стэйтменты, например. Специальный поток *Binlog Dump Thread* ожидает запросов от реплик и отдает бинлог им.

**Relay log** - реплика запускает IO-поток, который читает binlog с мастера и сохраняет его в локальный аналог binlog на реплике. Затем SQL-поток читает relay log и повторяет операции локально на реплике, соблюдая порядок и транзакционные границы.

### Binlog main properties

`binlog-format` - binlog format.
- `STATEMENT` - keeps changes as statements.
- `ROW` - structured representation of changes. Required for [[Debezium]] to work.  
- `MIXED` - uses both approaches depending on queries.

`binlog-row-image` - defines which data are dumped to binlog.
- `FULL` - all changes are dumped. Required for [[Debezium]] to work.
- `NOBLOB` - BLOB and TEXT are not dumbed if not changed.
- `MINIMAL` - only updated fields and identifiers involved in search are dumped. 

### SBL VS RBL

**Statement-Based Logging**
**Плюсы:**
- Меньше объём binlog - экономия диска и пропускной способности.
- Меньше нагрузка на запись binlog, так как просто записывается строка запроса.
- Быстрее при простых запросах, особенно `INSERT` и `UPDATE` по индексу.
**Минусы:**
- Возможны проблемы с детерминированностью (например, `NOW()`, `RAND()`, `UUID()`, `LIMIT`, `ORDER BY` без `WHERE` и т.д.).
- Репликация может привести к расхождениям, если состояние данных на мастере и слейве хоть немного различается.
- В целом, считается менее надежным, чем `ROW` и постепенно убирается из поддержки: `ROW` сделан дефолтным с версии MySQL 5.7.7; в Avrora `STATEMENT` планируется к депрекэйту.

Производительность обычно выше, особенно при большом количестве простых запросов.

**Row-Based Logging**
**Плюсы:**
- Максимальная точность. Репликация гарантированно будет верной, так как реплика получает конкретные значения, а не запрос.
- Лучше подходит для сложных запросов, использующих `triggers`, `stored procedures`.
**Минусы:**
- Огромные бинлоги, особенно при массовых `UPDATE` или `DELETE`.
- Более высокая нагрузка на диск и сеть.
- Более сложная интерпретация бинлогов.

Производительность обычно ниже из-за объёма логируемых данных, особенно при большом количестве модифицирующих запросов.

### Binlog configuration properties 
  
- `sync_binlog` - this MySQL property defines how often binlog is dumped to disk (N = each n-th transaction, or up to OS if 0), so we can set it to 0 or something else for the replica we read from and keep it 1 on master to save consistency on the same level and provide better performance at the same time.  
- `innodb_flush_log_at_trx_commit` - redo log commit and fsync. 0 - redo log and fsync once per sec, 1 - redo log and fsync every commit. 2 - redo log every commit, fsync every second.  
- `binlog_group_commit_sync_delay` - time in microseconds to batch transactions and fsync them together. Used with `sync_binlog=1` only.
- `binlog_cache_size` - the size of the memory buffer to hold changes to the binary log during a transaction. Default value is `32768`. If `SHOW GLOBAL STATUS LIKE 'Binlog_cache_disk_use'` shows non-zero values, cache has to be increased to avoid slow disk IO for binlog caching.
- `binlog_stmt_cache_size` - the size of the memory buffer for the binary log to hold nontransactional statements issued during a transaction. Default value is `32768`. If `SHOW GLOBAL STATUS LIKE 'Binlog_stmt_cache_disk_use'` shows non-zero values, cache has to be increased to avoid slow disk IO for binlog caching.
- `binlog-do-db`, `binlog_ignore_db`
	- **SBL**: write or ignore statements only when the default db (the one selected by `USE`) is the value of this property. To specify more databases use these options multiple times.
	- **RBL**: write or ignore statements for the database defined by these properties, independently from what is the default database (the one selected by `USE`).

https://dev.mysql.com/doc/refman/8.4/en/replication-options-binary-log.html

### Replication and Triggers

With statement-based replication, triggers executed on the master also execute on the slave. With row-based replication, triggers executed on the master do not execute on the slave. Instead, the row changes on the master resulting from trigger execution are replicated and applied on the slave. This behavior is by design.

If you want triggers to execute on both the master and the slave, you must use SBL. However, to enable slave-side triggers, it is not necessary to use SBL exclusively. It is sufficient to switch to SBL only for those statements where you want this effect, and to use RBL the rest of the time. You can do it on the session level with the following commands:
- `SET SESSION binlog_format = 'ROW';`
- `SET SESSION binlog_format = 'STATEMENT';`