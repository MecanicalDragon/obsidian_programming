There are 2 approaches exist to find objects, available for GC:

**Reference counting** GC algorithms associate a reference count with each object. These algorithms consider an object to be alive if the number of references to that object is greater than zero. This approach tends to stash garbage when several garbage objects point to each other and are considered as alive. JVM has no GC algorithms based on reference counting.

**Tracing collectors** determine the objects' reachability by tracing them from a set of root objects, known as *GC roots*. If an object is reachable from a root object, either directly or indirectly, then it will be considered alive, others are candidates for GC. Starting from the GC roots, the algorithm traverses the object graph recursively until there are no more objects left to visit. In the end, it considers all not visited objects unreachable and candidates for collection.

**GC roots** are objects that are surely alive. JVM considers the following GC roots:
- Local variables or anything stack frames are referring to right now
- Alive threads
- Static variables
- Classes loaded by the system classloader
- JNI locals and globals

All existing GCs balance among 3 core parameters:
- **Throughput** - percentage of consumed processor time
- **Latency** - STW time
- **Footprint** - amount of occupied space

All GCs divide memory to *generations* (serial\parallel\CMS) or to *regions* (Shenandoah, G1, ZGC) and are able to return memory back to the OS if they don’t need it anymore.

**Memory overheads of different GCs with a 2Gb heap**

| GC         | Overhead | % of overhead |
| ---------- | -------- | ------------- |
| Serial     | 7        | 0,3%          |
| Shenandoah | 36       | 1,8%          |
| Parallel   | 86       | 4,2%          |
| CMS        | 141      | 6,9%          |
| G1         | 165      | 8,1%          |
| ZGC        | 206      | 10,1%         |
