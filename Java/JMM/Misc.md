**Dangling pointers** (or wild pointers) in JMM are pointers that do not point to a valid object of the appropriate type. These are special cases of memory safety violations. Dangling references and wild references do not resolve to a valid destination.

---
**Live data** in the heap is data that is stably persists after the GC. It equals to the lower value of the Heap memory chart 'saw'. Safety formula: `Max Live Data < 50% of Xmx`
