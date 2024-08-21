Unsafe allows us to:
1. Altering private, final, static fields
2. Throwing checked exception without denoting it
3. Create objects without constructor invoke
4. Allocate and manipulate off-heap memory
5. Perform CAS-operations
6. Park/unpark threads (enhanced wait/notify methods)
7. Directly access CPU and other hardware

Unsafe.park is pretty much the same as thread.wait, except that it's using architecture specific code (thus the reason it's 'unsafe'). unsafe is not made available publicly but is used within java internal libraries where architecture specific code would offer significant optimization benefits. It's used a lot for thread pooling.

The problem with Unsafe is it allows you to manipulate memory directly and do a lot of things bypassing java rules, and these workarounds are much faster than provided by java ways to do it, that’s why developers want to use it. Which is why java wants to get rid of it. All modern frameworks use Unsafe this or that way, that’s why that’s not so simple. As a step to Unsafe decommission in Java 9 VarHandles were presented.

[Oracle Blog about Unsafe](https://blogs.oracle.com/javamagazine/post/the-unsafe-class-unsafe-at-any-speed)
