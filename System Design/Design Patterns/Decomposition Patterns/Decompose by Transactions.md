**This one is also called [[2-Phase Commit|Two-Phase Commit Pattern]].**
You can decompose services over the transactions. Then there will be multiple transactions in the system. One of the important participants in a distributed transaction is [transaction coordinator](https://www.baeldung.com/transactions-across-microservices).

The distributed transaction consists of two steps:
- **Prepare phase** — during this phase, all participants of the transaction prepare for commit and notify the coordinator that they are ready to complete the transaction
- **Commit or Rollback phase** — during this phase, either a commit or a rollback command is issued by the transaction coordinator to all participants

The problem with 2PC is that it is quite slow compared to the time for operation of a single microservice. Coordinating the transaction between microservices, even if they are on the same network, can really slow the system down, so this approach isn’t usually used in a high load scenario.
