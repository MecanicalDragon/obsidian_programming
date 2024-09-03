**Interpretation** – simple consequential execution of program code, command by command.

**AOT-compilation** (ahead of time, static) – process of programming language text into [native](https://itsobes.ru/JavaSobes/kak-vyzvat-nativnyi-kod/) machine code transformation. Langs like C++ use it. JDK can do AOT compilation with [jaotc](https://docs.oracle.com/en/java/javase/13/docs/specs/man/jaotc.html) utility.

**JIT-compilation** (just in time, dynamic) – “smart interpretation”. JVM analyzes bytecode and optimizes frequently called methods. This approach allows to write platform-independent code; also, it is one of the causes to warm up JVM before performance testing.

Java code compiled with `javac` turns the code into low-level bytecode. It is named a bytecode because every operation is encoded with a single byte. Approximately 200 operations exist in Java 10. Initially bytecode is interpreted in JVM command by command, but in runtime JVM can compile it in platform dependent machine code.

Since the very beginning Java has 2 JIT compilers:
- **C1 (Client compiler)** - designed to work fast and produce less optimized code. The client compiler fits better for desktop applications since we don't want to have long pauses for the JIT-compilation.
- **C2 (Server Compiler)** - requires more time and memory to compile but produces a better-optimized code. The server compiler is better for long-running server applications that can spend more time on the compilation.

Before Java 7 you used to have to select a compiler (-Xint, -client, -server) to run an application with just an interpreter, C1, or C2. Since Java 7 JVM has 5 compilation levels:
- level 0 - interpreter
- level 1 - C1 with full optimization (no profiling)
- level 2 - C1 with invocation and backedge counters
- level 3 - C1 with full profiling (level 2 + MDO)
- level 4 - C2

Since Java 7 HotSpot uses a compilation strategy called **tiered compilation**, which implies gradual switch from interpreter to C2. Java program, compiled with `javac`, starts its execution in an interpreted mode. The JVM tracks each called method and compiles it with C1. If the number of every concrete method call increases, the JVM recompiles these methods once more with C2.

`-XX:-TieredCompilation` flag disables intermediate compilation tiers (1, 2, 3), so that a method is either interpreted or compiled at the maximum optimization level (C2). This flag also decreases the number of compiler threads, the compilation policy, and the default code cache size (that will be 5 times smaller). Furthermore, a simple compilation policy, based on method invocation and backedge counters, will be chosen instead of an advanced compilation policy.

The unit of work for the HotSpot compiler is a loop or a method. This unit is called nmethod (native method). JVM compiles code in a parallel mode. Code that should be compiled enqueued in a priority queue. The higher counter of method invocations – the higher priority. Priority can be decreased with a time flow. Nmethod will be compiled in its turn; until that JVM uses bytecode to invoke it. C1 and C2 have their own queues. JVM stores method profiling data in MDO (method data objects) and uses it to apply the following code optimizations:

- Inlining of a hot method is allowed if the method’s size is less than 325 bytes.
- Escape-analysis allows checking if frequently created objects can not be seen outside the method and creating them in processor registers or stack frames to save heap memory and reduce GCs.
- Escape-analysis allows combining consequential synchronized sections that use a single monitor (lock coarsening), removing nested synchronized locks or synchronized locks from unreachable (out of scope) objects (lock elision).
- Unwinding of the cycle allows merging several cycle iterations in a single iteration because each iteration costs additional overhead on processor time.
- Monomorphic dispatching allows optimizing method calls when the object type is the same on every call. It assumes that the object type won’t change and avoids searching by virtual methods table every time we call the method.
- Intrinsic methods that are optimized platform native implementations of most critical methods (templates for x86_64 reside in `hotspot/src/cpu/x86/vm/x86_64.ad`).

JVM can perform code deoptimization if compiled or optimized code meets previously omitted branches. In this case, JVM deletes it and starts to interpret it again. Objects that use this are called non-entrant and will be removed.

JDK 8 ignores `-client`, `-server` and `-d64` flags, JDK 11 fails with `-d64` flag.
`-XX:TieredStopAtLevel=1` disables C2 compiler and uses C1 only.
`-Djava.compiler=NONE` or `-Xint` disables all compilers and executes a program in interpreter mode only.

Info about compilation logging can be found in [Habr part 1](https://habr.com/ru/post/536288/) article. More args for tuning can be found in [Habr part 2](https://habr.com/ru/post/536514/) article.

Source: [Aotc1](https://tproger.ru/news/jaotc-wtf/) and [aotc2](https://www.baeldung.com/ahead-of-time-compilation); [compilation in 4 parts](https://julio-falbo.medium.com/); [openjdk8](http://hg.openjdk.java.net/jdk8u/jdk8u/hotspot/file/2b2511bd3cc8/src/share/vm/runtime/advancedThresholdPolicy.hpp#l34)
