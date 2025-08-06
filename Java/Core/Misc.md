**Classpath** is a list of locations which can be deemed by the program as a source root directories for classes and resources lookup. Can be set during program launching with -cp argument, followed by the set of directories, separated by colon (Linux) or semicolon (win). In spring-boot jar archive classpath list is denoted in *classpath.idx* file, which is specified in **MANIFEST**.

---
**Unboxing** of the wrapped primitive value happens when arithmetic operators and comparison operators appear. But when `==` appears, it depends. If 2 boxing types are compared, it will compare references. But if the primitive appears on one side, and the other side has a boxed type, the boxed type will be unboxed to a primitive. In shorten conditions with `java.lang.Boolean` like `if (isDone)` unboxing happens, therefore you must be careful with null values, because NPE is possible.

---
`Comparator.compare(T o1, T o2)` / `Comparable.compareTo(T another)`. If method returns:
- a positive int — o1 gt o2 / caller gt another.
- zero — elements are equal
- a negative int — o1 lt o2 / caller lt another.

---
**Self-referential generic / Self-bound type / Recursive generic** - construction in a form of `EnumᐸE extends EnumᐸEᐳᐳ` that is useful when you’ve got an hierarchy of classes and want heirs to work with their specific objects being returned from parent methods or accepted as an argument without losing a specific type.

---
**Try block doesn't affect performance** and doesn’t change compiled instructions at all. It creates an entry in the exception table associated with the method. This table has a range of source instruction counters, an exception type, and a destination instruction. When an exception is raised, this table is examined to see if there is an entry with a matching type, and a range that includes the instruction that raised the exception. If it does, execution branches to the corresponding destination number. The important thing is that this table isn't consulted (and has no effect on running performance) unless it's needed. (Neglecting little overhead in the loading of the class.)

---
**Java’s parallelStream and CompletableFutures** use a common application’s ForkJoinPool under the hood. It may lead to a problem: if this pool is busy with another task, parallelStream will be waiting for its ending, yet that task may be long running… Fortunately, we can create a custom ForkJoinPool with necessary parallelism level and submit our streaming task there. The only thing – we need to shut down this pool manually when it finishes, because it won’t do it by itself, that will lead to memory leak. ([example](https://blog.krecan.net/2014/03/18/how-to-specify-thread-pool-for-java-8-parallel-streams/))

---
**MDC** — фича логгеров, позволяющая добавлять данные для логирования прямо в аппендер. Работает на тредлокалах, поэтому нужно быть осторожным с консистентностью данных. В реакторе для этого используется специальный `Closeable` ресурс. Удобно это тем, что с его помощью легко автоматизировать агрегацию логов, используя *correlation Id* в едином предустановленном в аппендере паттерне.

---
**Compact Strings**. Before JDK 9 Strings were encoded in UTF-16 and were char arrays within. Since JDK 9 they can be just byte arrays if there are no UTF-16 symbols contained in the String.
