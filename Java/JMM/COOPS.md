**COOPS** stands for **Compressed Ordinary Object Pointers**.<br>
In JVM data in memory are aligned by *machine words* that in a 64-bit system are equal 8 bytes and in a 32-bit system are equal 4 bytes. Which is why, the address always has 3 zero bits at the end (2^3 = 8). That way appears a possibility to use these 3 bits somehow. If during address saving, encode it (pointer >> 3), also during address extracting, decode it (pointer << 3), we can obtain 35-bit pointers! (But it works only for programs with heap size up to 32 GB). With coops turned on, following pointers being compressed:<br>
- Class word in object header.
- Objects of class field.
- Elements of object arrays.<br>

JVM objects are never being compressed. Compressing goes on JVM-level, but not bytecode level.<br>
OFC, coop-usage has an obvious flaw: time to access objects is increased, especially it affects GC.<br>
Usage of coops allows the usage of 32 GB of heap space with 32-bit references, but it's also possible to use compressed pointers when Java heap sizes are greater than 32GB. Although the default object alignment in Java Object Layout system is 8 bytes, this value is configurable using the `-XX:ObjectAlignmentInBytes` tuning flag. The specified value must be a power of two and must be within the range of 8 and 256. So, if we specify object alignment as 16 bytes, we can use up to 64 GB of heap space with coops.<br>
[Article on Baeldung about object layout](https://www.baeldung.com/java-memory-layout)
