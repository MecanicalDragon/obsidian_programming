Postgres handles some anomalies throwing errors. This behavior must be foreseen in the application by proper exception handling.

**Repeatable Read** isolation level avoids **lost updates** (hard case) returning `ERROR: could not serialize access due to concurrent update` for writing transactions, so the app must be capable to retry transactions that failed with this error. For ReadOnly transactions it is not applicable.

**Serializable** isolation level avoids **Serialization Anomaly** returning `ERROR: could not serialize access due to read/write dependencies among transactions`, so the app must be capable to retry transactions that failed with this error. This error can't occur on replicas, also to avoid it Serializable isolation level must not be combined with other isolation levels in a single application, otherwise it will unpredictably downgrade to the Repeatable Read time by time.

[Postgres Anomalies](https://mkdev.me/posts/transaction-isolation-levels-with-postgresql-as-an-example)

**SELECT FOR UPDATE** acquires exclusive lock over the rows. Supposed that this type of lock prevents even reading from another transactions, but in Postgres it allows other transactions to read these rows with just `SELECT`; it prevents only `SELECT FOR UPDATE`.

**SELECT FOR SHARE** acquires shared lock over the rows and allows other transactions read but not update these rows.

`WARNING`: in Postgres (and OracleDB at least) exists one mismatch between JPA contract and its database implementation: JPA's `LockMode.PESSIMISTIC_WRITE` lock just adds `FOR UPDATE` to all not-native queries. But Postgres’s `FOR UPDATE` doesn’t prevent simple `SELECT` from other transactions – just `UPDATE` and `SELECT FOR UPDATE` only.

[POSTGRES LOCKS](https://postgrespro.ru/docs/postgresql/9.6/explicit-locking)

[PostgreSQL Isolation Levels](https://www.postgresql.org/docs/current/transaction-iso.html).
[Postgres Advisory Locks](https://habr.com/ru/company/tensor/blog/488024/)
[Postgres MVCC](https://habr.com/ru/company/postgrespro/blog/442804/)
