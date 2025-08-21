**Strong Reference** – regular Java reference. Object referred with it can be finalized when there are no more strong references refer to it.

**Soft Reference** – object referred with it can be finalized if there are no more strong references refer to it, and JVM feels memory lack. All soft-reachable objects will be finalized before throwing **OutOfMemoryError**. This reference type is good for caching objects.

**Weak Reference** – ‘weak’ object will be finalized during the next GC if there are no strong or soft references pointing to it anymore, regardless of the available memory. This type of reference is good for metadata.

**Phantom Reference** – finalization alternative. It can be recycled by the GC at any time, just as no reference is associated with it. ‘Phantom’ object stored to the **ReferenceQueue** after finalization only, but before physical removal. Phantom reference cannot be created without ReferenceQueue; its *get()* method always returns null.

