**InnoDB** - engine for **MySQL**

**Redo log** - журнал восстановления InnoDB, аналог WAL в Postgres. Отвечает за восстановление бд после краша. В нем хранятся физические изменения страниц данных,
информация, необходимая для "переигрывания" завершённых транзакций после сбоя. При старте MySQL, если был крэш, — InnoDB "переигрывает" транзакции из redo log.

**Binlog** - источник правды для репликации и логгирования всех изменений, *Point-in-time recovery*. Мастерский бинлог читают реплики для соблюдения консистентности. Хранит данные в более высокоуровневом формате: стэйтменты, например.
### Redo and binlog MySQL properties

`binlog-format` - binlog format.
- `STATEMENT` - keeps changes as statements.
- `ROW` - structured representation of changes. Required for [[Debezium]] to work.  
- `MIXED` - uses both approaches depending on queries.

`binlog-row-image` - defines which data are dumped to binlog.
- `FULL` - all changes are dumped. Required for [[Debezium]] to work.
- `NOBLOB` - BLOB and TEXT are not dumbed if not changed.
- `MINIMAL` - only updated fields and identifiers involved in search are dumped. 
### Performance improvement ideas  
  
- use separated volume for binlog to exclude excess access to the file storage  
- read from replica  
- `sync_binlog` - this MySQL property defines how often binlog is dumped to disk (N = each n-th transaction, or up to OS if 0), so we can set it to 0 or something else for the replica we read from and keep it 1 on master to save consistency on the same level and provide better performance at the same time.  
- `innodb_flush_log_at_trx_commit` - redo log commit and fsync. 0 - redo log and fsync once per sec, 1 - redo log and fsync every commit. 2 - redo log every commit, fsync every second.  
- `binlog_group_commit_sync_delay` - time in microseconds to batch transactions and fsync them together. Used with `sync_binlog=1` only.