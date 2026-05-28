**LockSupport** is built around `park`/`unpark` methods of [[Unsafe]] and provides a facade to use it, picking a virtual thread implementation of these methods for *Virtual Threads*.

The most interesting method of LS is `setCurrentBlocker` that assigns a synchronization object to a thread. This object allows inner JVM structures to see the reason why the thread is parked (good practice to pass there an object that is a real reason of the thread parking). It is useful in the following scenarios:
- In a thread dump you see the reason of thread parking that eases deadlocks investigation.
- Libraries can use it for better planning of the high-load-related structures like below:
	- Virtual Thread Scheduler is able to reschedule tasks among alive threads using it.
	- Monitoring systems can see the critical resource before everything stuck on it.

Method `Unsafe.park(Object blocker)` uses `setCurrentBlocker` to set a blocker before the `park` method and reset if after. But in high-loaded systems `setCurrentBlocker` may be useful to set the blocker once before the `park` call in the loop, that reduces overhead.
