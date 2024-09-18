An **oop**, or **ordinary object pointer** is a pointer to an object in memory; it is normally the same size as a native machine pointer, or [[Memory Layout|machine word]]. On a 32-bit system they take 4 bytes (32 bits); on a 64-bit system they take 8 bytes (64 bit). With oops JVM is able to manage 4 GB of memory on ILP32 systems, and approximately 1.5 times as large on LP64 systems.

**COOPS** stands for **Compressed Ordinary Object Pointers**. Coops represent 64-bit pointers as 32-bit values in PL64 or 32-bit pointers as 35-bit pointers in ILP32. This allows applications to address up to 32Gb of the heap memory. At the same time, data structure compactness in LP64 systems is competitive with ILP32 mode. So, how does it work?

The precision of addressing in JVM is 8 bytes. JVM adds padding to the objects so that their size is a multiple of 8 bytes. With these paddings, the last three bits in oops are always zero (2^3 = 8). That way appears a possibility to use these 3 bits somehow. If during address saving, encode it (pointer >> 3), also during address extracting, decode it (pointer << 3), we can obtain 35-bit pointers with a possibility to address up to 32 GB of the heap. When the maximum heap size is more than 32 GB, the JVM will automatically switch off the oop compression, but we can still address 32GB+ of the heap with [[Memory Layout#^padding|custom padding]].

Hotspot JVM has 2 implementations of coops: *narrow pointers* and *zero-based pointers*. To decode a narrow pointer it must be scaled by a factor of 8 (pointer << 3) and added to a 64-bit *address base* to find the object they refer to. Coops use an arbitrary address for the *narrow oop base* which is calculated as *java heap base* minus one (protected) page size for implicit NULL checks to work. *Zero-based coops* are used when JVM succeeds to make *narrow oop base* zero. They have 2 benefits over the simple coops: no excess calculations with base and avoiding of additional NULL checks, which are both good for performance.

Zero based implementation tries to allocate java heap using different strategies based on the heap size and a platform it runs on. First, it tries to allocate java heap below 4Gb to use compressed oops without decoding if heap size < 4Gb. If it fails or heap size > 4Gb it will try to allocate the heap below 32Gb to use zero based compressed oops. If this also fails it will switch to regular compressed oops with narrow oop base.
## What pointers are compressed?

In an ILP32-mode JVM all oops are the native machine word size. In a LP64-mode `UseCompressedOops` flag by default is turned on, and the following oops in the heap are be compressed:
- the classword in object header of every object
- every oop (object reference) in object instance fields
- every element of an oop (object) array

Never compressed:
- JVM data structures to manage Java classes. These are generally found in the section of the Java heap known as the Permanent Generation (PermGen). 
- In the interpreter, oops are never compressed. These include JVM locals and stack elements, outgoing call arguments, and return values. The interpreter eagerly decodes oops loaded from the heap, and encodes them before storing them to the heap.
- Method calling sequences, either interpreted or compiled.
- In compiled code, oops are compressed or not according to the outcome of various optimizations.

Structures in compiled code that can refer to either compressed oops or native heap addresses:
- register or spill slot contents
- oop maps (GC maps)
- debugging information (linked to oop maps)
- oops embedded directly in machine code (on non-RISC chips like x86 which allow this)
- nmethod constant section entries (including those used by relocations affecting machine code)

Important C++ values which are never compressed:
- C++ object pointers (_this_)
- handles to managed pointers (type Handle, etc.)
- JNI handles (type jobject)

[OpenJDK wiki](https://wiki.openjdk.org/display/HotSpot/CompressedOops)
[Coops on Baeldung](https://www.baeldung.com/jvm-compressed-oops)

