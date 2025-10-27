MySQL Blackhole engine allows creation of tables that don't store any data. That's it, if you `SELECT` from such table you always get empty result; all side effects of such insertions, updates and deletes take place though. This can be useful for replication or audit purposes, or for performance testing (I/O load is extremely low owing to no real data manipulation).

[[MySQL Binlog|Binary log]] is written for blackhole-engined tables, but you have to remember that `ROW` binary log replication replicates only inserts since owing to blackhole tables don't store any real data there is no possibility to log previous state. Which is why if it is necessary to log updates and deletes `STATEMENT` replication must be opted.

Triggers are also executed on the blackhole tables, but since such tables don't store real data, in fact, again, only `INSERT` statement triggers take any effect. How does it happens? MySQL has 2 layers: *SQL layer* and *Storage engine layer*.
- SQL Layer parses queries, build query plans, calls triggers, prepares row list for the change.
- Storage engine Layer is responsible for the physical data storage and modification.
Triggers are handled on the SQL Layer. Layer calls engine for all `OLD` rows before the update, but since blackhole engine doesn't keep any rows, no triggers will be executed for updates/deletes.

`AUTO_INCREMENT` in blackhole tables has its own nuances.
- Table doesn't store any data; and although `AUTO_INCREMENT` still returns values, the underlying sequence will be reset after the database restart.
- If `STATEMENT` based replication is used, autoincremented values for master and replica will diverge (replica will use its own autoincrement). For `ROW` based replication it will work (for inserts only, again).
