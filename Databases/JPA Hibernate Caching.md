### Cache Concurrency Strategies

- **READ_ONLY** – can be used only for entities that never change.
- **NONSTRICT_READ_WRITE** – cache updates when a transaction that updates data is committed. Strong consistency is not guaranteed.
- **READ_WRITE** – guarantees strong consistency, achieves it with so-called soft locks that lock entities before an update and release after the commit has been performed.
- **TRANSACTIONAL** – cache updates are performed in distributed transactions. Cached entity update is either committed or rollbacked in both cache and database in a single transaction.

### Cache levels

1. *1 level* is tied to the Hibernate Session, cannot be disabled. Caches an object by its id. Load-method doesn’t really load data before it is requested. Consequent loads in a single transaction load data from Session cache but not from database itself.
2. *2 level* tied to Hibernate SessionFactory that works between sessions. Must be enabled in properties. Caches entities annotated with `@Cache`. Entity’s dependent entities must be annotated either if you want to cache it too. 2 level cache is queried only if the entity wasn’t found in 1 level cache.
3. *Query cache* by default is disabled too and must be enabled in properties. The key to the cache is a summary set of query parameters. Caches the result of HQL query if the query is explicitly marked as cacheable.

