**Shenandoah** is a low-latency oriented GC. It reduces pauses by doing most of the work concurrently with the user program. Shenandoah divides the heap area into multiple equal-sized regions. A region is a unit of memory allocation or reclamation. Furthermore, it adds an additional word to the object layout – *Indirection Pointer* or *Brooks Pointer*, that allows moving objects without updating all existing references to them.  Shenandoah performs object relocation and compaction concurrently, making use of different special barriers in different phases, like the *SATB* barrier, read barrier, and write barrier. If free memory is over, Shenandoah goes to one of the failure modes: pacing, degenerated collection, and in the worst case, a full collection.

Shenandoah has several properties to configure Garbage Collection:

- **Modes** define the way Shenandoah runs and which barriers it uses. Three available modes:
	- *normal/SATB* (default) – concurrent CG, SATB marking. This marking mode is similar to what G1 is doing: intercepts writes and marks "previous" objects.
	- *IU* - concurrent GC with *Incremental Update* marking. This marking mode is the mirror of SATB mode: intercepts writes and marks "new" objects.
	- *passive* - runs stop-the-world GCs

- **Heuristics** determine when GC should start and which regions it should evacuate.
	- *adaptive* (default) - observes the previous GC cycles, and tries to start the next GC cycle so that it could complete before heap is exhausted
	- *static* (aka dynamic) - decides to start GC cycle based on heap occupancy
	- *compact* - runs GC cycles continuously, starting the next cycle as soon as previous cycle finishes, as long as allocations happen
	- *aggressive* - tells GC to be completely active. It will start the new GC cycle as soon as the previous one finishes (like *compact*), and it will evacuate all live objects
### GC cycle

**Marking** mostly happens in concurrent mode. Shenandoah uses the *SATB* algorithm and makes use of the SATB barrier to maintain the SATB view of the heap. Anyway, an *initial root marking* and a *final-mark* require STW. The *final-mark* drains all pending queues, re-scans the root set and prepares the *collection set* that indicates regions ready for evacuation. Once the marking is complete, garbage regions (regions where no live objects are present) are ready to be reclaimed. This cleanup happens concurrently.

**Evacuation** step implies moving live objects mentioned in the *collection set* to other regions. It is done to reduce the fragmentation in memory allocation and also is known as *compact*; it happens completely concurrently. Shenandoah achieves it by performing CAS updates of the objects’ *Brooks pointers* to objects’ new location. Also, it uses the read and write barriers to perform strict copy of objects.

**Reference Update** phase is mostly concurrent and implies traversing through the heap and update of the object references that were moved during the evacuation. Two brief periods in this phase require the STW: *init-update-refs* initializes the update reference phase, and *final-update-refs* updates the root set and recycles regions mentioned in the *collection set*.

[Habr](https://habr.com/ru/post/681256/)
