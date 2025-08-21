In JVM every object consists of *object header* and *object fields*. Object header consists of *mark word* and *class word* - both are usually [[Memory Layout|machine words]]. Arrays also have so-called *length word* that contains information about array length. Sometimes (in LP64 system when `UseCompressedOops` is turned on that is true by default) there could also be a 32-bit gap field that is often available to store instance fields.

Class word stores pointer on the class itself in the [[Java Native Memory Tracking#^7c8d1d|compressed class space]]; mark word’s content depends on the object's state. 

**32-bit JVM header structure**
![](object_header_32.png)

**Object can be in 1 of 5 states:**
## Normal state (biased_lock = 0, lock = 01)

In this state 25 bits of the markword are filled with the object's *identity hash code* if it's been calculated already, because the calculation is lazy.

**Identity hash code** is a value that’s returned by default when an object’s *hashCode()* method is called, if it is not overridden. In Hotspot OpenJDK it can be calculated with the following ways, depending on a value passed to a function:

- 0. Random generated number.
- 1. Function of object’s address in memory.
- 2. Always 1 (used in sensitivity testing).
- 3. Sequence.
- 4. Object’s address in memory, represented as an int value.
- 5. Thread state, merged with xorshift ( [https://en.wikipedia.org/wiki/Xorshift](https://en.wikipedia.org/wiki/Xorshift) )

In Java 8-9 the default way is 5.
In Java 6-7 the default way is 0.

Other 7 bits are:

- **lock** – contains lock state:
	- 00 — Lightweight Locked,
	- 01 — Unlocked or Biased,
	- 10 — Heavyweight Locked,
	- 11 — Marked for Garbage Collection.
- **biased_lock** – contains ‘1’ if biased locking is turned on for this object, ‘0’ otherwise.
- **age** – shows the number of outlived GC’s. When it reaches the ‘max-tenuring-threshold’ number, the object moves to OG.
## Biased Locked (biased_lock = 1, lock = 01)

**Biased locking** is used since HotSpot6 and turned on by default; it is deprecated since Java 17 due to excess complexity. 

Object locking is expensive for CPU because it uses expensive [[CAS-FAS-FAA|CAS]] instructions. But the vast majority of the objects, once created, are usually used by the same single thread, hence atomic instruction usage is wasteful. Which is why biased locking allows threads to bind objects to themselves and skip CAS operations, increasing performance. Biased lock bit determines if an object is bound to a thread, whose id is denoted in the markword. Hotspot does not enable biased locking right after the JVM startup (currently four seconds).

When a locked object is first fetched by a thread, the thread uses CAS operation to put its ID into the object’s markword with ‘1’ as a bias locking bit. In future, when the thread enters and exits the synchronization block, it does not require CAS operations to lock and unlock, it just simply checks if the markword in the object header stores its ID. If the check succeeds, it means the thread already possesses the object. When there is another thread trying to acquire the lock, the bias mode is declared to be over. In this state markword instead of identity hash code keeps values:

- **thread** - id of the thread that possesses the object (23 bits).
- **epoch** – time indicator of how long the owner-thread possesses the object (2 bits).

Since there is no place to store identity hash code in this state, once calculated, it eternally forbids biased locking for this object. Theoretically, it can be a useful feature: in case of frequent race conditions we can turn off biased locking just invoking identity hash code calculation, increasing performance this way. Anyway, this can be useful only before java 17, because since java 17 biased locking is deprecated due to its complexity.
## Lightweight Locked (lock = 00)

Since context switching among threads consumes resources of the processor, if race condition is insufficient, it’s more profitable for the processor not to switch thread context, but go into a small busy loop, called *Spin*. **Spin lock** was introduced in JDK 1.4.2 and is used in lightweight locks. It’s the more useful, the less waiting time, otherwise context switching is better. Lightweight locks use CAS operations to avoid the overhead of using *mutual exclusive locks* (*mutexes*).

When the code enters the synchronization block, JVM allocates a space named *lock record* in the stack frame of the current thread. The markword from the locked object’s header is being copied to the lock record; it is known as *displaced markword*. After that, JVM uses CAS operation to replace locked object's markword with a pointer to the lock record. If this update fails several times, that means that multiple threads compete for a lock. In this case lightweight lock expands to a heavyweight lock, and the thread that waits for the lock goes into a blocking state.

**ptr_to_lock_record** – reference to a lock record, space where mark word has been moved.

Since in this state only one thread tries to possess the object, this pointer points to a space in this thread’s stack. That’s good for 2 reasons:
- Performance increased because of the absence of race conditions.
- That’s enough for the thread to know if it’s the one who holds the lock.
## Heavyweight Locked (lock = 10)

If the time of race condition is significant, lightweight lock is replaced with heavyweight lock that uses an object monitor under the hood. **Object monitor** is a real object in the heap that holds the mark word of the master object and two sets of waiting threads.

When multiple threads access synchronized code at the same time, first one assigns itself to the owner variable in object monitor, the others park in the *entrySet* of the monitor. If the thread calls the `wait()/sleep()` method or finishes execution of the critical section, it releases the lock (sets owner variable back to null), so that other threads can unpark and capture the lock. If the thread calls the `wait()/sleep()` method in the critical section, it parks to the *waitSet* of the monitor and waits there for some other thread calls `notify()` or `notifyAll()`. After that, one or all threads in the *waitSet* will be transferred to the *entrySet* and will compete for lock in common order. Since only the heavyweight monitor object has *waitSet* and *entrySet*, calling `wait()` method automatically turns other locks to heavyweight.

**ptr_to_heavyweight_monitor** – reference to the object monitor.
## Marked for GC (lock = 11)

Nothing to describe here.

## Locks Summary

When an object is first instantiated, it is biased, meaning that there is only one thread that can use it. That thread uses CAS instruction to store its ID to the object’s markword during first access and doesn’t need to do it again during other accesses; it is only necessary for it to compare IDs to know if it’s the owner of the object.

Once another thread accesses this object, it can see the object's bias state that indicates that there is already a competition for the object. If the thread that initially held the object's lock is terminated, the object goes into a lock-free state, and then can be re-biased to a new thread; if the original thread is still alive and requires that object, then the bias lock is promoted to a lightweight lock.

Lightweight lock is optimistic and believes that competition exists, but its severity is low, so the awaiting thread doesn’t wait actually, but goes into a small waiting cycle called a *spin* until another thread releases the lock. But when the spin exceeds a certain number of cycles, or a third thread requires this lock, the lightweight lock expands to a heavyweight lock that is pessimistic and parks all waiting threads in the *entrySet* of the monitor object, preventing the processor from idling.

**64-bit JVM with coops header structure**
![](object_header_64.png)
