Index is a database structure that improves data retrieving speed, but also decreases data adding time. *B-Tree* indexes are most common and wide-used. Generally, indices can be *clustered* and *non-clustered*.

**Clustered index** sorts and stores table or view entries based on their keys - columns that are included in index’s definition. There only one clustered index can exist for every table, because entries in the table can be stored in a single way. If the table has a clustered index, all entries of this table are sorted according to it, otherwise entries in the table are unsorted.

**Nonclustered index** has its own structure that is distinct from the table. Nonclustered index contains a key and a pointer to an entry in the related table. This pointer structure depends on the related entry table. If the table has a clustered index, the pointer points to its key, pointing to an entry’s row otherwise.

**Indexes can be:**
- **Unique index** - ensures that a single entry exists for specified parameters set.
- **Covering index** - contains all queried information.
- **Composite index** - indexes several table columns.
- **Particular index** - covers only that part of all entries that makes sense for the index. This can reduce index structure size.

Indexes work better for more selective data sets, which means the lower the number of entries satisfying the condition, the higher the index's effectiveness. Vice versa, the more data wrapped in an index condition, the fewer index effectiveness, so the optimizer can even decide not to use it in the query.

Columns in a composite index should appear in an order that makes the most sense for the queries to select data. Index (ABC) is usable for all queries that select data by (A), (AB) and (ABC). A single table can have multiple indexes with the same params if their order differs.

In **OracleDB** indexes can be:
- **Usable** or **unusable**. Unusable index is not maintained by DML and is ignored by optimizer
- **Visible** or **invisible**. Invisible index is not maintained by DML and by default is not used by optimizer.
