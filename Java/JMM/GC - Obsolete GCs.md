### Serial GC

First of all, because of the *Weak Hypothesis of Generations* that says that the longer an object lives, the less possibility of its death, Garbage collectors divide heap into *New Generation* and *Old Generation* (Tenured) parts.

NG takes 1/3 of all heap space, 2/3 belong to *OG*. *Eden* takes 8/10 of NG, and almost all objects are created here. Huge objects (*Accelerates*) are exceptions and born directly in OG. 2/10 of NG take *Survivor1* and *Survivor0* (1/10 each).

When *Eden* fills up, *STW* (Stop-The-World) happens. During *STW* all alive objects from *Eden* relocate to *S0*, then *S0* swaps with *S1*, and *Eden* is purged. When next time Eden fills up, all alive objects from it and S1 relocate to S0.

When created objects outlive the configured number of GCs (8 be default), they are moved to *OG*. When one of the survivors becomes full, all alive objects relocate to OG. When OG fills up, the *Mark-Sweep-Compact* (MSC) process purges the heap during one large STW. The *MSC* process goes in 3 steps: mark alive objects, delete garbage, compact alive objects.
### Parallel GC

This collector is the same as Serial. The only difference – *MSC* goes simultaneously in several threads, each of them takes care of its own area of the heap. It’s faster, but the disadvantage of this approach is fragmented heap space. In Java 8 this GC is default.
### CMS

Concurrent Mark Sweep collector was removed from Java, because he had not got any significant advantages. Its *NGGC* was happening as usual, but *OGGC* was scheduled and was going concurrently with program execution. *OGGC* flow was like:
- STW (initialize roots)
- alive objects search
- STW (marking of rest alive objects again)
- purge.
*Floating Garbage* is a term that appears in this kind of GC. It is objects that became garbage between root marking and purge. They can be collected during the next CG cycle only.
### Epsilon

Epsilon is a no-op garbage collector. It handles memory allocation but does not implement any actual memory reclamation mechanism. Once the available Java heap is exhausted, the JVM shuts down. There are some cases when we know that the available heap will be enough, so we don't want the JVM to use resources to run GC tasks. Some examples of such cases:

- Performance or memory pressure testing
- VM interface testing
- Extremely short-lived jobs
- Last-drop latency or throughput improvements
