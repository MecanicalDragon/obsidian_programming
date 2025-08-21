**ZGC** is a single-generation, low-latency collector, designed for very large heap sizes — up to 16TB. Z-collector aims at STW periods of 10 ms., does most of its work concurrently and takes advantage of 64-bit pointers with *reference coloring* – technology that affords to remap objects using extra information stored in the pointer. This approach reduces memory fragmentation, but also has an obvious drawback: *ZGC works only on 64-bit platforms*. ZGC performs object relocation concurrently with an application operation and uses *load barriers* to check *colored bits* of the reference and avoid application access to the relocated object. ZGC calls it *remapping*.

### GC cycle: Marking phase

Marking phase in ZGC is divided into three steps:

- During a short STW pause ZGC marks root references.
- Concurrently ZGC traverses the object graph, starting from the GC roots, and marks every object it reaches. At the same time, when a load barrier detects an unmarked reference, it marks it too.
- During the STW ZGC handles some edge cases like weak references.
###### Reference Coloring and Multimapping

ZGC keeps the reference state in the bits of the reference itself. It's called *reference coloring*. To avoid different references pointing to the same object, ZGC uses *multimapping*. That means that multiple addresses in virtual memory point to the same address in physical memory. ZGC operates more virtual memory than physical JVM heap size, because one physical object can be accessed through different virtual memory spaces, thanks to colored bits in its reference. ZGC reference uses 42 bits to represent the address itself; on top of that, there are 4 bits to keep the reference state. These bits are called metadata bits, exactly one of them is 1:

- **finalizable bit** – the object is only reachable through a finalizer
- **remap bit** – the reference is up to date and points to the current location of the object
- **marked0** and **marked1** bits – used to mark reachable objects

### GC cycle: Relocation phase

Relocation phase in ZGC consists of the following steps:

- A concurrent phase, which looks for blocks we want to relocate, and makes an entry in the *relocation set*.
- An STW phase relocates all root references from the *relocation set* and updates their references.
- A concurrent phase relocates all remaining objects in the *relocation set* and stores the mapping between the old and the new addresses into the *forwarding table*.

The rewriting of the remaining references happens in the next marking phase. With this approach we don't have to traverse the object tree twice. Meanwhile, *load barriers* can do it as well.
###### Load Barriers and Remapping

In the relocation phase we haven’t updated most of the references to the relocated objects. ZGC uses *load barriers* to handle this issue. Load barriers update the references pointing to relocated objects with a technique called *remapping*. When application loads a reference, it triggers a load barrier, which follows next steps to return the correct reference:

1. Checks whether the remap bit is set to 1. If so, it means that the reference is up to date, so it can safely return it.
2. Checks if the referenced object was mentioned in the *relocation set*. If it wasn't, that means we didn't want to relocate it. To avoid this check next time we load this reference, we set the *remap bit* to 1 and return the updated reference.
3. Object was the target of relocation. The only question is whether the relocation happened or not? We’re looking for a corresponding entry in the *forwarding table*. If we find it, we go to the next step. Otherwise, we relocate the object now and create an entry with a new address in the *forwarding table*. Then we proceed with the next step.
4. Now the object was surely relocated - either by ZGC in the previous step, or the load barrier during an earlier hit of this object. We update the initial reference to the new location of the object (either with the address from the previous step or by looking it up in the forwarding table), set the *remap bit* to 1 and return the reference.

And that's it, since every time we load a reference ZGC triggers the load barrier, and we can be sure that each time we try to access an object we get the most recent reference to it. This approach decreases application performance, especially the first time we access a relocated object. But this is a price we have to pay if we want short pause times.
