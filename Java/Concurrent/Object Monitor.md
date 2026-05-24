**Object monitor** is a real object in the heap that holds the [[Object Headers|mark word]] of the master object (*displaced header*), owner thread variable, and two sets of waiting threads:
- *entrySet* - a set of threads competing for the lock.
- *waitSet* - a set of threads called `wait()` in the critical section.

When multiple threads access synchronized code at the same time, first one assigns itself to the *owner variable* in object monitor, the others park in the *entrySet* of the monitor. If the thread calls the `wait()` method or finishes execution of the critical section, it releases the lock (sets owner variable back to null), so that other threads in the entrySet could unpark and capture the lock. If the thread calls the `wait()` method in the critical section, it parks to the *waitSet* of the monitor and waits there for some other thread calls `notify()` or `notifyAll()`. Then the thread (or all threads) in the *waitSet* are transferred to the *entrySet* and compete for the lock in common order, but acquiring it they proceed from the last executed instruction in the synchronized section, not from the beginning of it.

Since a thread called `wait()` can be awaken multiple times before the condition it waits for occurs, the correct usage pattern is not `if` but `while`:
```java
synchronized (lock) {
	while (!condition) {
		lock.wait();
	}
}
```
Presence of these 2 sets in the object monitor is an architecture mistake: object monitor combines 2 different abstractions:
- **mutual exclusion** (monitor ownership, *entrySet*)
- **condition synchronization** (wait/notify, *waitSet*)
These are two different concurrency tasks. Later they will be segregated in interfaces [[Locks|Lock]] and `Condition`: `Condition c = lock.newCondition();`

- `wait()` can be called only within the critical section, or `IllegalMonitorStateException` will be thrown.
- Calling `sleep()` in the critical section doesn't release the lock.

---
`notify` and `notifyAll` were introduced long ago, when resources consumption for thread awakening and context switching was much more significant, and these 2 methods made sense. Nowadays `notify` is considered dangerous, because `notify` awakes a random thread.
If multiple threads are waiting for different conditions on the same monitor, there may occur a situation when thread whose condition is not fulfilled is awaken by `notify` rather than a thread whose condition is fulfilled, and there is no other thread who can call `notify` again, so the program can halt.

