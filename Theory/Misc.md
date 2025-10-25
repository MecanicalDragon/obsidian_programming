
**Tail recursion** is a recursion made by the last expression in a function. Its advantage is that the most compilers (including JVM) can expand such a recursion in a cycle, excluding the stack overflow possibility.

---
**Pure function** is a function that is *determined* and has *no side effects*. Determined means that for the same input data function always returns the same result. Side effect absence means that function doesn’t affect its input parameters or objects outside its scope.

---
**Authentication** is a process of user identity check. ^a1

**Authorization** is a provisioning of permissions for specific actions to the user, also verification of such permissions when the user attempts to perform these actions. ^a2

---
**Classic Singleton** is an antipattern because it has at least two responsibilities: its primal responsibility and the creation of the singleton itself. Furthermore, it is static, that means its implementation is tightly coupled with all its consumers and can not be removed in unit testing or anywhere else. It has a global state, is not visible in class dependencies, increases coupling, and provokes to write ugly code.

---
**Amdahl's Law**
If you divide a task to several parallel subtasks, total execution time can’t be less than execution time of the slowest subtask. Also, runtime speed improvement of the program by its parallelizing is limited with a time required on its sequential instructions execution.

---
**Law of Demeter** is a specific case of *Loose Coupling*. Each program component must:
- Be aware of a limited amount of other elements - only those that have close relation to it.
- Interact only with known elements.
- Interact only with themselves, objects, created by themselves, their method parameters or its own inner components.