### ACID stands for the requirements to the transaction system.

**Atomicity** – a transaction will be committed in the system completely (all data will be saved) or no data will be saved at all.
**Consistency** – transaction, that reaches its end, can save only allowable data into the database.
**Isolation** – during transaction flow no other transactions can affect one’s result.
**Durability** – independently of lower level problems, committed changes remain saved in the database.

### SQL:92 standard determines 4 types of data anomalies and 4 levels of transaction isolation:

**Lost Update** – data updates come simultaneously from 2 different transactions, and one of the updates overrides the another one.
**Dirty Read** – data not committed yet, but another `T` already sees the update.
**Non-repeatable Read** -  `T` rereads the same row and sees it changed by another `T`.
**Phantom Read** – `T` reruns query and finds additional rows committed by another `T`. ^anomalies

**Read Uncommitted** - `T` is able to read uncommitted changes of another `T`.
**Read Committed** - `T` sees only committed changes but for every query in the same `T` database state is refreshed.
**Repeatable Read** - `T` makes a DB snapshot at the start, so DB state is same for every query in the same `T`. This level is also called **Snapshot Isolation**.
**Serializable** - eliminates all possible anomalies. Most databases use range locks to ensure that, and it costs performance.

|                      | Dirty Read               | Fuzzy Read | Phantom Read             |
| -------------------- | ------------------------ | ---------- | ------------------------ |
| **Read Uncommitted** | possible (avoided in PG) | possible   | possible                 |
| **Read Committed**   | avoided                  | possible   | possible                 |
| **Repeatable Read**  | avoided                  | avoided    | possible (avoided in PG) |
| **Serializable**     | avoided                  | avoided    | avoided                  |
### Anomalies beyond SQL:92

**Read Skew** — Two *different* queries within T1 read data updated by T2 in between. The difference with *Fuzzy Read* is FR queries the same row, this one gets an anomaly querying different rows.
**Write Skew** — Two transactions running in parallel read the current state and make decisions based on it. Each of them independently doesn’t break constraints, but together they do, because their decisions are based on obsolete data, that was valid on T start. For example, in a clinic there is a constraint that at least one doctor should be on-call. Two doctors read the state at the same time. D1 sees D2 is on-call and D2 sees D1 is on-call, both request day-off and get approvals from the system. In result no doctors on-call at all - the constraint is violated.
**Read-Only Transaction Anomaly** — 2 transactions update the same related data in different rows (T1 adds interest income to the bank account 1 based on all accounts balance; T2 decreases balance of bank account 2). One transaction commits data, another one doesn't. Then T3 reads the state of accounts balance. In any case any state that T3 can see will be inconsistent. [About it](https://johann.schleier-smith.com/blog/2016/01/06/analyzing-a-read-only-transaction-anomaly-under-snapshot-isolation.html).
**Serialization Anomaly** — 2 transactions update data and each of them affects results of another one's query. T1 updates all false-values to true, T2 updates all true-values to false. ^anomalies2

| Isolation/Anomaly | Read Skew | Write Skew | ROTA     | SA       |
| ----------------- | --------- | ---------- | -------- | -------- |
| Read Uncommitted  | Possible  | Possible   | Possible | Possible |
| Read Committed    | Possible  | Possible   | Possible | Possible |
| Repeatable Read   | Avoided   | Possible   | Possible | Possible |
| Serializable      | Avoided   | Avoided    | Avoided  | Avoided  |
### Lost Updates

**Lost Update** can be of 2 different types:
- easy one is when the data had been updated by T1 and then overwritten by T2 before T1 is either committed or rolled back. This one is not allowed under all isolation levels and can be avoided using atomic updates (`set x = x+1`).
- hard one that is a special case of the write skew. It happens when T1 reads data into a local memory and then updates it after it was modified by T2. This one is not allowed under Repeatable Read and above and can be avoided using explicit locks (`select for update`).

Both these types can be avoided in databases automatically under proper isolation levels.

### See Other:
- [[Postgres Anomaly Error Handling]]
- [[MVCC]]

