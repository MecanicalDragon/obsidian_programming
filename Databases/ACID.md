### ACID stands for the requirements to the transaction system.

**Atomicity** – a transaction will be committed in the system completely (all data will be saved) or no data will be saved at all.
**Consistency** – transaction, that reaches its end, can save only allowable data into the database.
**Isolation** – during transaction flow no other transactions can affect one’s result.
**Durability** – independently of lower level problems, committed changes remain saved in the database.

### SQL:92 standard determines 4 types of data anomalies and 4 levels of transaction isolation:

**Lost Update** – data updated by 2 transactions simultaneously, yet 1 of updates has been overridden by another one.
**Dirty Read** – data not committed, yet already read by another transaction.
**Fuzzy Read** (non-repeatable) - when a transaction rereads data, it sees this data changed by another transaction.
**Phantom Read** – a transaction reruns query and finds additional rows committed by another transaction.

**Read Uncommitted** - T can read uncommitted changes of another T.
**Read Committed** - T reads only committed changes but for every query in a single T DB state is refreshed.
**Repeatable Read** - T makes a DB snapshot on its start, so DB state is same for every query in a single T. This level is also called **Snapshot Isolation**.
**Serializable** - forbids all possible anomalies. Most DB use range locks to guarantee that, and it costs performance. Postgres however uses so-called **Serializable Snapshot Isolation (SSI)** which uses Snapshot Isolation with some locking and checks to ensure serializable guarantees.

Oracle DB doesn’t support Serializable level at all because of its low performance and calls Snapshot-isolation level as Serializable. That’s why Oracle's serializable level doesn’t prevent writes from other transactions.

|                                          | Dirty Read               | Fuzzy Read | Phantom Read             |
| ---------------------------------------- | ------------------------ | ---------- | ------------------------ |
| **Read Uncommitted**                     | possible (avoided in PG) | possible   | possible                 |
| **Read Committed**                       | avoided                  | possible   | possible                 |
| **Repeatable Read (Snapshot Isolation)** | avoided                  | avoided    | possible (avoided in PG) |
| **Serializable**                         | avoided                  | avoided    | avoided                  |

### Anomalies beyond SQL:92

**Read Skew** — Two *different* queries within T1 read data updated by T2 in between. The difference with Fuzzy Read is FR queries the same row, this one gets an anomaly querying different rows.
**Write Skew** — Two transactions running in parallel read the current state and make decisions based on it. Each of them independently doesn’t break constraints, but together they do, because their decisions are based on obsolete data, that was valid on T start. For example, in a clinic there is a constraint that at least one doctor should be on-call. Two doctors read the state at the same time. D1 sees D2 is on-call and D2 sees D1 is on-call, both request day-off and get approvals from the system. In result no doctors on-call at all - the constraint is violated.
**Read-Only Transaction Anomaly** — 2 transactions update the same related data in different rows (T1 adds interest income to the bank account 1 based on all accounts balance; T2 decreases balance of bank account 2). One transaction commits data, another one doesn't. Then T3 reads the state of accounts balance. In any case any state that T3 can see will be inconsistent. [About it](https://johann.schleier-smith.com/blog/2016/01/06/analyzing-a-read-only-transaction-anomaly-under-snapshot-isolation.html).
**Serialization Anomaly** — 2 transactions update data and each of them affects results of another one's query. T1 updates all false-values to true, T2 updates all true-values to false.

| Isolation/Anomaly | Read Skew | Write Skew | ROTA     | SA       |
| ----------------- | --------- | ---------- | -------- | -------- |
| Read Uncommitted  | Possible  | Possible   | Possible | Possible |
| Read Committed    | Possible  | Possible   | Possible | Possible |
| Repeatable Read   | Avoided   | Possible   | Possible | Possible |
| Serializable      | Avoided   | Avoided    | Avoided  | Avoided  |
### Lost Updates

**Lost Update** can be of 2 different types:
- easy one is when the data that has been updated by T1 is overwritten by T2 before T1 is either committed or rolled back. This one is not allowed under all isolation levels and can be avoided using atomic updates (`set x = x+1`).
- hard one that is a special case of the write skew. It happens when T1 reads data into a local memory and then updates it after it was modified by T2. This one is not allowed under Repeatable Read and above and can be avoided using explicit locks (`select for update`).

Both these types can be avoided in databases automatically under proper isolation levels.
