The memory used by the Java process includes:<br>
- **Thread stacks** – 80-200 kb per thread, 150 kb average.
- **Java Heap** – this is where most of the java objects live.
- **Metaspace** – virtual space, divided into chunks. Every chunk belongs to the sole classloader, but single classloader can possess several chunks. When the classloader dies, it exempts its chunk. Metaspace has at least one subspace:
	- **CompressedClassSpace** (3GB max usually, 1GB default) – class metadata (method bytecodes, symbols, constant pools, annotations) are stored here.  CompressesClassPointers – Java optimization that works like compressed oops; it makes class references 32-bit.
- **GC memory** – additional memory for heap management (~+10% of heap):
	- **Mark bitmap** - map of reachable objects
	- **Mark stack** - the structure for traversing the reachable object graph during marking phase of GC
	- **Remembered sets** – sets of inter-region references
- **Code Cache** – JIT-compiled methods (nmethods, native methods) and all the generated code, default size 2496 Kb, 240Mb – default limit with tiered compilation on, 48 Mb otherwise. Since Java 9 code cache is divided into three parts:
	- **JVM internal (non-method code)**. JVM machine code (interpreter, for example). Size depends on a number of compiler threads. Size can be determined with `-XX:NonNMethodCodeHeapSize`.
	- **Profiled code**. Partially optimized machine code with a short lifetime. Takes half of the space left after non-method code allocation. Can be defined with `-XX:ProfiledCodeHeapSize`.
	- **Non-profiled code**. Fully optimized code with potentially long lifetime. Takes half of the space left after non-method code allocation. Can be defined with `-XX:NonProfiledCodeHeapSize`.
- **Compiler** – JIT compiler itself also requires memory to do a compilation.
- **Symbols** – stores symbols such as field names, method signatures, and interned strings.
	- **SymbolTable** - hashtable that stores names, signatures, identifiers, field and method names.
	- **StringTable** - string pool, hashtable that stores metadata of interned Strings (references, hash codes). Before JDK 7 Strings were stored here too, but since JDK 7 Strings themselves are stored in the heap to avoid OOM. StringTable size:
		- Before JDK 7: 1009 buckets
		- JDK 7 - JDK 11: 60013 buckets
		- JDK 11+: 65536 buckets
	- **Runtime Constant Pool**. Compile-time numeric literals or method and field references, resolved at runtime.
- **Directly allocated** – usually don’t play a big role in total memory consumption:
	- **Direct buffers** - memory-mapped files, e.g., all JAR files on the classpath. ByteBuffer.allocateDirect()
	- **Unsafe.allocateMemory()**
	- **Native libraries**
<br>[Stack Overflow answer](https://stackoverflow.com/questions/53451103/java-using-much-more-memory-than-heap-size-or-size-correctly-docker-memory-limi)
<br>`-XX:NativeMemoryTracking=summary` – key that is needed to be included in JVM options to track consumed memory.<br>
`jcmd <jps> VM.native_memory` – command to check memory consumption.<br>
**Jconsole** – a tool that allows tracking memory consumption.<br>
