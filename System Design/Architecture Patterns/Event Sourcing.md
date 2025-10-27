Event sourcing solves the problem of atomic transaction commit and message dispatch. It suggests to persist the state of a business entity as a sequence of state-changing events. Whenever the state of a business entity changes, a new event is appended to the list of events. Since saving an event is a single operation, it is inherently atomic. The application reconstructs an entity’s current state by replaying the events.

Application persists events in an append-only event store. The event store also behaves like a message broker and provides an API that enables services to subscribe to events. When a service saves an event in the event store, it is delivered to all interested subscribers. This can simplify tasks in complex domains, by avoiding the need to synchronize the data model and the business domain, while improving performance, scalability, and responsiveness. It can also provide consistency for transactional data, and maintain full audit trails and history that can enable compensating actions.

Some entities can have a large number of events. In order to optimize loading, an application can periodically save a snapshot of an entity’s current state. To reconstruct the current state, the application finds the most recent snapshot and the events that have occurred since that snapshot. As a result, there are fewer events to replay.

**Benefits**
- It solves one of the key problems in implementing an event-driven architecture and makes it possible to reliably publish events whenever state changes.
- Because it persists events rather than domain objects, it mostly avoids data concurrency problems.
- It provides a 100% reliable audit log of the changes made to a business entity
- It makes it possible to implement temporal queries that determine the state of an entity at any point in time.
- Event sourcing-based business logic consists of loosely coupled business entities that exchange events. This makes it a lot easier to migrate from a monolithic application to a microservice architecture.

**Drawbacks**
- It is a different and unfamiliar style of programming and so there is a learning curve.
- The event store is difficult to query since it requires typical queries to reconstruct the state of the business entities. That is likely to be complex and inefficient. As a result, the application must use [[System Design/Architecture Patterns/CQRS]] to implement queries. This in turn means that applications must handle eventually consistent data.

- [more here](https://learn.microsoft.com/en-us/azure/architecture/patterns/event-sourcing)
- [and here](https://microservices.io/patterns/data/event-sourcing.html)
