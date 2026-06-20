- **Process** is a program in execution, it consumes resources of the OS (CPU, RAM). **Thread** is a unit of execution within a process.
- A process consists of multiple threads. A thread is the smallest part of the process that can execute concurrently with the other parts of the process (other threads).
- A process has its own memory space and can not corrupt the memory space of another process. Thread uses the process’s address space and shares it with other threads of that process.
- A thread can communicate with other threads of the same process directly by using shared memory and methods like *wait()*, *notify()*, *notifyAll()*. A process can communicate with other processes by using *IPC* ([inter-process communication](https://en.wikipedia.org/wiki/Inter-process_communication)).
- Threads are managed within the application. Processes are managed by the OS.
- Threads have control over the other threads of the same process. A process does not have control over the sibling process; process has control over its child processes only. A creation of new process requires duplication of the parent process.

## Context switching

Context switching is performed by the *OS Task Scheduler*. During the inter-process context switch OS preserves state of the current process in the *PCB* (Process Control Block) which contains information about process id and state, process counter, various registers, memory limits, list of open files, etc.), so the process could be restored and resume execution later. Then OS retrieves the state of another process from the *PCB*. It is expensive due to registers loading, memory pages switching, and kernel data structures updating.

Context switching between threads is cheaper because it requires fewer states to track; and since threads share one memory address space OS doesn’t have to switch virtual memory pages that is one of the most expensive operations during context switching.

[[Virtual Threads]] and [[Kotlin Coroutines|coroutines]] reduce context-switching overhead even more due to task scheduling on the application side.
