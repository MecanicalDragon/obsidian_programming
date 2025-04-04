**PersistenceContext** - object that manages entities.
**EntityManager** - object that allows us to communicate with *PersistenceContext*.

PersistenceContext can be:
- **Transaction-scoped** exists within a transaction, hence, you need a transaction to work with it.
- **Extended-scoped** exists independently of transaction or other contexts. That means that entities persisted in it are not available in other contexts, also you don't need a transaction to work with it. If you use extended context outside a transaction, saved entities are also saved only in context, not in storage. If you use it within a transaction, saved entities will be flushed into storage with the end of transaction.
