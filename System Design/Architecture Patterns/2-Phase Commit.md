2PC is a pattern in microservice architecture to implement distributed transactions.

In a two-phase commit protocol, there is a coordinator component that is responsible for controlling the transaction and contains the logic to manage the transaction. The other component is the participating nodes (e.g., the microservices) that execute their local transactions. 2PC protocol executes a distributed transaction in two phases:

- **Prepare Phase**: The coordinator asks the participating nodes whether they are ready to commit the transaction. The participants return *yes* or *no* answers and prepare to do their part of the transaction if necessary.
- **Commit Phase**: If all the participating nodes responded affirmatively in phase 1, then the coordinator asks all of them to commit. If at least one node returns negative, the coordinator asks all participants to roll back their local transactions.

If a coordinator decides to confirm a transaction during 2 phases, it must achieve transaction completeness with any ways, even if other participants or coordinator itself goes down. Hence, it will bombard participants with requests until they all confirm transaction commit, also it must be able to proceed transaction coordination after restart if it goes down itself. Regardless of all problems with 2PC, it looks like it remains *the only way* to provide distributed transaction atomicity if there are more than one unrollbackable local transaction participants.

**Problems with 2PC**
- Coordinator node can become the single point of failure
- All other services need to wait until the slowest service finishes its confirmation. Thus, the overall performance of the transaction is bound by the slowest service
- 2PC is slow by design due to the chattiness and dependency on the coordinator. Thus, it can lead to scalability and performance issues in a microservice-based architecture involving multiple services
- 2PC protocol is not supported in NoSQL databases. Thus, in a microservice architecture where one or more services use NoSQL databases, a two-phase commit cannot be applied
- *Exactly once* semantics is not possible and you should use at least once, that means all services should be idempotent
- If after the 1st phase transaction coordinator or the application server goes down, participating nodes don't know what to do (cannot commit and cannot rollback either). So, until the coordinator comes back and completes the 2nd phase, the participating data stores will remain in a locked state. This inhibits performance. Worse things happen when the transaction log gets corrupted after the 1st phase. In that case, the data stores remain in the locked state forever.
