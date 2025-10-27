**Command Query Responsibility Segregation**. Once we implement database-per-service, we need to collect and join data from multiple services that may be encumbering. [CQRS](https://docs.microsoft.com/en-us/azure/architecture/patterns/cqrs#event-sourcing-and-cqrs) suggests splitting the application into two parts — the command side and the query side.

- The command side handles the Create, Update, and Delete requests
- The query side handles the query part by using the materialized views

The event sourcing pattern is generally used along with it to create events for any data change. Materialized views are kept updated by subscribing to the stream of events.
### See Other
- [[System Design/Architecture Patterns/CQRS|CQRS]]
