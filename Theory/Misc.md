
**Tail recursion** is a recursion referenced from the last expression in a function. Its advantage is that the most compilers (including JVM) can unfold such a recursion into a cycle, avoiding the stack overflow possibility.

---
**Pure function** is a function that is *determined* and has *no side effects*. Determined means that for the same input function always returns the same result. Side effect absence means that function doesn’t modify its input parameters or objects outside its scope.

---
**Authentication** is a process of user identity check. ^a1

**Authorization** is provisioning of permissions for specific actions to the user, also verification of such permissions when the user attempts to perform these actions. ^a2

---
**Classic Singleton** is an antipattern because it has at least two responsibilities: its actual business responsibility and creation of the singleton itself. Furthermore, it is static, hence, its implementation is tightly coupled with all its consumers and can not be removed in unit testing or anywhere else. It has global state; it is not visible in class dependencies; it increases coupling, and provokes to write ugly code.

---
**Amdahl's Law**
Program runtime speed up is limited by the the execution part that can't be parallelized.
This law depicts that infinite parallelization can't bestow infinite speed improvement, and at some point bottle neck optimization gives more speed up than further parallelization.

---
**Law of Demeter** or **The least knowledge principle**
Object should know as little about other object inners as possible. Object may call only:
- methods of its own
- methods of its fields
- methods of parameters (arguments)
- methods of objects created by itself
