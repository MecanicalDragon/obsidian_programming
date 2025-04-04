**Advantages over statement**
- You can reduce code quantity using the same statement in different places. This decreases the error chance.
- Before the query execution DB parses it and builds an execution plan. It is a heavy operation, so DB caches the query as it is – with parameters, and remembers its execution plan. PS usage allows DB to cache PS instead and use its execution plan for same queries with other parameters in the future.
- Prepared statements protect against SQL-injections. PS itself and data specified in it are transferred in the database separately. Parameters data are stored in DB as variables. So, there is no possibility to change or corrupt query with SQL-injection.
