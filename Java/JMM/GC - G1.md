**G1** uses a different approach to garbage collection, than serial\parallel\CMS. It was supposed to work with applications with heap size more than 4GB and low and predictable latency. G1 arranges a heap in a different way too. Heap is divided into multiple regions of the equal size – usually 1 - 32MB per region. The Maximum number of regions is 2048. The exception is *humongous regions* – they consist of several regular regions to contain humongous objects. Humongous objects are objects that take more than a half of the region. They are treated in a special way:

- They can never be moved among regions.
- They can be removed during the marking cycle or full GC.
- They are always alone in the region.

Division of regions to Eden, Survivor and Tenured is only logical for G1 – regions can even change their affiliation. *NGGC* goes the same way as usual – several threads do it during the *STW*. But the difference is – it happens only with a part of all regions at the same time, to complete in a preconfigured timespan.
### GC cycle: Young-only

G1 performs only *NGGCs*. When heap occupancy exceeds a threshold (45% of the heap by default), G1 starts the *marking cycle*, that goes simultaneously with an application:
1. **Initial mark**. Root marking with STW
2. **Concurrent mark**. All alive objects are marked simultaneously with an application operation. G1 uses *Snapshot-At-The-Beginning* (*SATB*) algorithm: alive objects are objects that were alive at the beginning + new objects. *Floating garbage* is possible in this approach too. Also, G1 prepares the *collection set* - set of regions to reclaim space from if it is reasonable. 
3. **Remark**. Additional search of alive objects with STW. Empty regions reclamation.
4. **Cleanup**. Reclamation of humongous regions. Clean support structures of reference accounting during the STW, determine whether a space-reclamation phase will actually follow. 
### GC cycle: Space Reclamation

After the end of the *marking cycle* G1 switches to a *Mixed GC mode*: simultaneously with every *NGGC* G1 exempts some OG regions. G1 performs garbage collections and space reclamation in the STW pauses and makes use of collected statistics to select regions that can be cleaned not exceeding preconfigured time. Live objects are promoted to other regions in the heap, and existing references to these moved objects are adjusted. 

- Objects of the young generation (eden and survivor regions) are copied into survivor or old regions, depending on their age. 
- Objects from old regions are copied to other old regions.
- Objects in humongous regions are never moved, but G1 can reclaim the space they occupy if they’re dead.

After enough memory cleaning G1 switches back to the regular mode. If there are no free regions for alive objects, *allocation failure* happens. G1 stops the entire application and cleans garbage all over the heap. Using its collected statistics G1 can change the number of regions for every generation to optimize future GCs.

**Advantages**: precise prediction of STW time, absence of heap fragmentation.
**Flaws**: 10% of throughput consumption.

[Config parameters](https://blog.gceasy.io/2020/06/02/simple-effective-g1-gc-tuning-tips/)
[Oracle blog](https://docs.oracle.com/en/java/javase/17/gctuning/garbage-first-g1-garbage-collector1.html#GUID-F1BE86FA-3EDC-4D4F-BDB4-4B044AD83180)
