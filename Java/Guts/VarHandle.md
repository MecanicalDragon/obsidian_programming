**VarHandle** class provides access to variables under specific conditions. The goal of VarHandle is to define a standard for invoking equivalents of `java.util.concurrent.atomic` and `sun.misc.Unsafe` ([[Unsafe]]) operations on fields and array elements. Those operations are mostly atomic or ordered operations — for example, atomic field incrementation.

VarHandle methods allow access to variables under specific memory ordering effects.
- *Plain* reads and writes guarantee bitwise atomicity only for references and primitives under 32 bits. Also, they impose no observable ordering constraints with respect to the other threads.
- *Opaque* operations are bitwise atomic and coherently ordered with respect to access to the same variable.
- *Acquire* and *Release* operations obey Opaque properties. Furthermore, Acquire mode reads, and their subsequent accesses are ordered after matching Release mode writes and their previous accesses.
- *Volatile* operations are fully ordered with respect to each other.

Generally, all the methods of the VarHandle class fall to five different access modes.
1. *Read Access* - allows getting the value of the variable under specified memory ordering effects. There are several methods with this access mode like: `get()`, `getAcquire()`, `getVolatile()` and `getOpaque()`.
2. *Write Access* - allows setting the value of the variable under specific memory ordering effects. Similarly, to methods with read access, we have several methods with write access: `set()`, `setOpaque()`, `setVolatile()`, and `setRelease()`.
3. *Atomic Update Access* – `compareAndSet()` and others.
4. *Numeric Atomic Update Access* – `getAndAdd()` and others.
5. *Bitwise Atomic Update Access* – methods with this access allow us to atomically perform bitwise operations under specific memory ordering effects: `getAndBitwiseOr()` method, for example.

It’s very important to remember that *access modes will override previous memory ordering effects*. This means that, for example, if we use `get()`, it will be a plain read operation, even if we declared our variable as `volatile`.

[Oracle documentation](https://docs.oracle.com/javase/9/docs/api/java/lang/invoke/VarHandle.html)
