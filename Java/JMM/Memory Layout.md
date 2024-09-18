JVM works with memory in the context of *machine words*. **Machine word** is a minimal data fragment that can be read or written by JVM. Its length depends on a CPU architecture and is equal to 32 or 64 bits depending on ILP32 or LP64 system. <u>Machine word tiering is forbidden in JVM</u>. To comply with machine word borders JVM adds paddings to object layout. Because of those a single field couldn't spread to more machine words that it minimally requires.

JVM is able to pack several small data types into a single machine word, but these data will be tightly coupled and couldn't be read or updated independently - JVM couldn't acquire access over only a part of machine word or write exactly those bytes that small data type takes.

Since JVM can't operate less bytes than machine word takes, small data types packed together can't be handled by different threads reliably without bad memory effects, because the machine word is the minimal unit thread can acquire access on.

Even more, since modern CPUs actively use *cachelines* that are usually 64/128 <u>bytes</u> nowadays, more unfavorable memory effects in multithreaded environment are possible. Which is why annotation `@Contended` was presented. It adds paddings around annotated field to prevent data update loss because of cache line usage. By default, these paddings are 128 bytes, but it is possible to control it with -`XX:ContendedPaddingWidth` tuning flag.

JVM is able to shuffle object fields order to compact them reducing necessary paddings and make data to take less space.

Object padding is configurable using the `-XX:ObjectAlignmentInBytes` flag. The specified value must be a power of two and must be within the range of 8 and 256. So, if we specify object alignment as 16 bytes, we can use up to 64 GB of heap space with [[COOPS]]. ^padding

Java Object Layout (JOL) - a tool that shows can show how Java aligns object field data in memory.
`org.openjdk.jol:jol-core`.

[Article on Baeldung about object layout](https://www.baeldung.com/java-memory-layout)
