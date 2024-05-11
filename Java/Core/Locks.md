**ReentrantLock** – permits only one thread to acquire the lock. Reentrancy means that this lock can be acquired several times by a single thread without negative consequences.
<br>**ReadWriteLock** – has 2 types of locks:
- **ReadLock** can be acquired by several threads.
- **WriteLock**, being acquired, doesn’t allow acquiring readLock, but being not acquired waits for obtaining in the same queue with readLocks.
<br>**StampedLock** – being acquired returns a *long* value, that represents a timestamp of the lock acquaintance; this value is used to unlock the lock. StampedLock has both of read and write locks, but also supports optimistic locking mechanism, that returns zero if the writeLock is acquired. You always need to validate received value when you access shared resource and convert readlock to writeLock if you want to change it.
