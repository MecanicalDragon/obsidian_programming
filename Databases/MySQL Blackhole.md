MySQL Blackhole engine allows creation of tables that don't store any data. That's it, all operations with it take 0.00 s; and if you `SELECT` from such table you always get empty result. This engine can be useful for replication or audit purposes, or for performance testing.

- The `BLACKHOLE` storage engine supports all kinds of indexes.
- The maximum key length is 3072 bytes.
- The `BLACKHOLE` storage engine does not support partitioning.

[[MySQL Binlog|Binary log]] is also written for `BLACKHOLE` tables. When performing binary logging, all inserts to such tables are always logged, regardless of the logging format in use. Updates and deletes are handled differently depending on whether *statement based* or *row based* logging is in use. With the SBL, all statements affecting `BLACKHOLE` tables are logged, but their effects ignored. When using RBL, updates and deletes to such tables are simply skipped - they are not written to the binary log. A warning is logged whenever this occurs.

Triggers are also executed on the blackhole tables, but since such tables don't store real data, in fact, again, only `INSERT` statement triggers take any effect. How does it happens? MySQL has 2 layers: *SQL layer* and *Storage engine layer*.
- SQL Layer parses queries, build query plans, calls triggers, prepares row list for the change.
- Storage engine Layer is responsible for the physical data storage and modification.
Triggers are handled on the SQL Layer. Layer calls engine for all `OLD` rows before the update, but since blackhole engine doesn't keep any rows, no triggers will be executed for updates/deletes.

`AUTO_INCREMENT` in blackhole tables has its own nuances. The engine does not automatically increment field values, and does not retain auto increment field state. This has important implications in replication if the source table is `BLACKHOLE` with `AUTO_INCREMENT PRIMARY_KEY`: 
- In SBL, the value of `INSERT_ID` in the context event is always the same. Replication therefore fails due to trying insert a row with a duplicate value for a primary key column.
- In RBL, the value that the engine returns for the row always be the same for each insert. This results in the replica attempting to replay two insert log entries using the same value for the primary key column, and so replication fails.
