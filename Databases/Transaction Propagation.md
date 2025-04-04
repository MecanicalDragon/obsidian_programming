
| Propagation            | If transaction exists already                                              | And if it doesn’t   |
| ---------------------- | -------------------------------------------------------------------------- | ------------------- |
| **REQUIRED** (default) | Business logic appends to existing Transaction                             | Creates new T       |
| **REQUIRES_NEW**       | Suspends current Transaction, creates new one                              | Creates new T       |
| **NESTED**             | Creates a SAVEPOINT; if further logic throws an exception, rollbacks to it | Creates new T       |
| **SUPPORTS**           | Uses existing Transaction                                                  | Doesn’t use T       |
| **NOT_SUPPORTED**      | Suspends current Transaction, performs logic without Transaction           | Doesn’t use T       |
| **NEVER**              | Throws an exception                                                        | Doesn’t use T       |
| **MANDATORY**          | Uses existing Transaction                                                  | Throws an exception |

*Nested Transactions* can work because of *savepoints* that were released with JDBC 3.0. Savepoints allow a transaction to mark a point it can rollback to.