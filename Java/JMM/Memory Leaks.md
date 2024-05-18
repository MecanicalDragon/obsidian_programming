**Memory Leak** is a situation when objects are not needed anymore but are still referred to, and GC cannot remove them. Memory Leak reasons:<br>
- static fields live unless classloader of the class becomes eligible for GC
- not closed resources consume memory
- improper *equals()* and *hashCode()* methods in HashMap
- inner non-static classes reference to the outer class instance
- overridden *finalize()* method can be a reason class cannot be GC-ted instantly
- before Java 7 long strings, being interned, stay in the pool
- *ThreadLocals* in modern web containers can stash garbage, because containers pool threads
