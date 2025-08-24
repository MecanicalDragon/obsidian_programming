- **Process** is a program in execution, it consumes resources of the OS (CPU, RAM). **Thread** is a unit of execution within a process.
- A process consists of multiple threads. A thread is the smallest part of the process that can execute concurrently with other parts (threads) of the process.
- A process has its own memory space and can not corrupt the memory space of another process. Thread uses the process’s address space and shares it with other threads of that process.
- A thread can communicate with other threads of the same process directly by using shared memory and methods like *wait()*, *notify()*, *notifyAll()*. A process can communicate with other processes by using *IPC* ([inter-process communication](https://en.wikipedia.org/wiki/Inter-process_communication)).
- Threads are managed within the application. Processes are managed by the OS.
- Threads have control over the other threads of the same process. A process does not have control over the sibling process; process has control over its child processes only. The creation of a new process requires duplication of the parent process.

### Context switching

Context switching is performed by the *OS Task Scheduler*. During the inter-process context switch OS preserves the state of the current process into *PCB* (Process Control Block), which contains the info about process id and state, process counter, different registers, memory limits, list of open files, etc.), so the process could be restored and resume execution later. Then OS retrieves the state of another process from *PCB*. It is expensive due to registers loading, switching memory pages, and updating kernel data structures.

Context switching between threads is cheaper because it requires fewer states to track; and since threads share one memory address space OS doesn’t have to switch virtual memory pages (that is one of the most expensive operations during context switching).

[[Virtual Threads]] and [[Kotlin Coroutines|coroutines]] reduce context-switching overhead even more due to task scheduling on the application side.
