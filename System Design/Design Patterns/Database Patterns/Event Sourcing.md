Most applications work with data, and the typical approach for the application is to maintain the current state. For example, in the traditional CRUD model a typical data process is to read data from the store. It contains limitations of locking the data with often using transactions.

The [Event Sourcing pattern](https://docs.microsoft.com/en-us/azure/architecture/patterns/event-sourcing) defines an approach to data operations handling that’s driven by a sequence of events; each of them is recorded in an append-only store. Application code sends a series of events that imperatively describe each action that occurred on the data to the event store, where they’re persisted. Each event represents a set of changes to the data.

The events are persisted in an event store that acts as the record storage. Typical uses of the events published by the event store are to maintain materialized views of entities as actions in the application change them, and for integration with external systems. For example, a system can maintain a materialized view of all customer orders that are used to populate parts of the UI. As the application adds new orders, adds or removes items on the order, and adds shipping information, the events that describe these changes can be handled and used to update the materialized view. The figure shows an overview of the pattern.

![[design_patterns_2.png]]

### See Other
- [[System Design/Architecture Patterns/Event Sourcing|Event Sourcing]]

