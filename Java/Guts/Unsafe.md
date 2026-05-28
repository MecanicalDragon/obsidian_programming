Unsafe allows us to:
1. Altering private, final, static fields
2. Throwing checked exception without denoting it
3. Create objects without constructor invoke
4. Allocate and manipulate off-heap memory
5. Perform CAS-operations
6. Park/unpark threads (enhanced wait/notify methods)
7. Directly access CPU and other hardware

The problem with Unsafe is it allows you to manipulate memory directly and do a lot of things bypassing java rules, and these workarounds are much faster than provided by java ways to do it, that’s why developers want to use it. Which is why java wants to get rid of it. All modern frameworks use Unsafe this or that way, that’s why that’s not so simple. As a step to Unsafe decommission in Java 9 [[VarHandle]] were presented.

## Park/Unpark

These methods implement a concept of so-called *permit* for threads. Technically, permit is a counter on the native C++ layer of the thread. A new thread has this counter equal to 0 (no permit). When thread A calls `unpark` for another thread B, thread B acquires permit (its counter becomes 1). The counter can't be more than 1, so subsequent calls of `unpark` for thread B don't increase it until thread B awakes.

When thread B calls `Unsafe.park()`, it checks the permit first. If the counter > 0, thread *consumes* the permit (decreases the counter) and proceeds. If the counter == 0, thread parks, and the following `unpark` for it will give permit to thread B and awake it with this. Permit will be consumed then and thread B will proceed.

This algorithm prevents a problem possible with old `Thread.suspend()` and `Thread.resume()` methods, when `resume` is called before `suspend` and the awakening signal for it losts forever.

`Unsafe.unpark(thread)` is "unsafe" solely because the caller must somehow ensure that the thread has not been destroyed. Nothing special is usually required to ensure this when called from Java (in which there will ordinarily be a live reference to the thread) but this is not nearly-automatically so when calling from native code. `park` is unsafe only because `unpark` is unsafe.

`Object.wait()` uses a monitor, thus the awaken thread is aware of changes made by other threads (respects [[Happens Before]] contract). `park/unpark` have no monitor and are not synchronized. They don't check memory updates, so you have to do it manually. The best way to do it - have a volatile variable that is written before `unpark` call and read after `park` exit. This makes 2 threads respect Happens Before contract.
- T1 `park` call Happens Before T1 `park` return.
- `unpark(T1)` Happens Before `T1 park()` return.

[Oracle Blog about Unsafe](https://blogs.oracle.com/javamagazine/post/the-unsafe-class-unsafe-at-any-speed)
