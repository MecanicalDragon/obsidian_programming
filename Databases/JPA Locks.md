JPA locks are a transaction alternative.

Database locks enable obtaining exclusive access to the data and do it in a more fine-grained manner than transactions. Generally, databases provide 2 types of locks:
- **Shared Lock**, being acquired, allows others to read, but not to write locked data. ^dbsharedlock
- **Exclusive Lock**, being acquired, forbids others from even reading. Required to modify the data. ^dbexclusivelock

JPA specification determines a types of locks.
- **Optimistic Lock** expects that data won’t be modified by another thread during operation and throws an exception in case it was.
- **Pessimistic Lock**, being acquired, doesn’t allow data modification by other threads.

To make use of optimistic locks we don't need to use any db lock but need the entity to have a mandatory `@Version` attribute that must be an integer or a timestamp. This attribute is automatically incremented every time the entry is updated. We must not modify it manually. 2 types of optimistic locks:
- **OPTIMISTIC / READ** prevents data from dirty and non-repeatable reads. An `OptimisticLockException` is thrown if another thread modifies data during the operation.
- **OPTIMISTIC_IMCREMENT / WRITE** does the same but also increments the `@Version` attribute of the entry.

JPA defines 3 types of pessimistic locks:
- **PESSIMISTIC_READ** obtains a *shared lock* and prevents data from being updated or deleted by other threads.
- **PESSIMISTIC_WRITE** obtains an *exclusive lock* and prevents data from being even read.
- **PESSIMISTIC_FORCE_INCREMENT** works likewise the *PESSIMISTIC_WRITE* and additionally increments a `@Version` attribute of the entry.

When we need data just to be read and not updated during operation, we should use *P_READ* lock. But if we need to update or delete data, we need to promote it to *P_WRITE* lock.

JPA assumes 2 lock scopes: *NORMAL* and *EXTENDED*. Normal lock is acquired only for the entry itself, while Extended one is acquired for all related entries either. Not all persistence providers support extended mode.

`lock.timeout` is a parameter that defines the time that we want to wait to obtain a lock until `LockTimeoutException` will be thrown.
