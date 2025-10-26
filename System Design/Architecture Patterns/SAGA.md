**Saga** architecture pattern provides transaction management using a sequence of local transactions. Every operation that is part of the Saga can be rolled back by a compensating transaction that must be idempotent and retriable to ensure that a transaction can be managed without any manual intervention. Saga pattern guarantees that all operations either  complete successfully or the corresponding compensation transactions undo the previously completed work. There are two approaches to implement the Saga pattern: *choreography* and *orchestration*.

### Choreography Pattern

In the Saga Choreography pattern each microservice that participates in a distributed transaction publishes an event that is processed by the next microservice. Choreography flow is successful if all the microservices complete their local transactions and there is no failure reported by any of them. In the case of failure, microservice sends a corresponding event, which is read by another participating service that runs a rollback transaction and resends the event further. Once again: a compensating transaction must be idempotent and retriable.

The Choreography pattern is suitable for greenfield microservice application development. Also, this pattern is suitable when there are fewer participants in the transaction. Following are a few frameworks to implement the choreography pattern:

- Axon Saga: A lightweight framework and widely used with Spring Boot based microservices
- Eclipse Micro Profile LRA: Implementation of distributed transactions for HTTP transport based on REST principles
- Eventuate Tram Saga: Saga orchestration framework for Spring Boot and Micronaut based microservices
- Seata: Open-source distributed transaction framework with high-performance and easy-to-use services

### Orchestration Pattern

In the Orchestration pattern, a single *Saga Execution Coordinator* (SEC) is responsible for managing the overall transaction status. If any of the microservice encounters a failure, then the orchestrator is responsible for invoking the necessary compensating transactions. SEC contains a Saga log that captures the sequence of events of a distributed transaction. For any failure, SEC inspects the Saga log to identify impacted components and the sequence in which the compensating transactions should execute. For any failure in the SEC component, it can read the Saga log once it’s coming back up. It can then identify which transactions successfully rolled back, which ones are pending, and can take appropriate actions.

The Saga orchestration pattern is useful for brownfield microservice development architecture, this pattern is suitable if we already have a set of microservices and would like to implement the Saga pattern in the application. We need to define the appropriate compensating transactions to proceed with this pattern. *Camunda* and *Apache Camel* are well suitable frameworks to implement this pattern.

How to provide atomicity of transaction commit and next step triggering?
- [[Event Sourcing]]
- [[Aggregate Pattern]]
- [[Transactional outbox]]
- [[WAL]]

---
[SAGA by Baeldung](https://www.baeldung.com/cs/saga-pattern-microservices)
[SAGA by microservices](https://microservices.io/patterns/data/saga.html)
[SAGA by Microsoft](https://docs.microsoft.com/ru-ru/azure/architecture/reference-architectures/saga/saga)
[SAGA by Habr](https://habr.com/ru/post/427705/)
[SAGA by jboss](https://jbossts.blogspot.com/2017/12/saga-implementations-comparison.html)
