**SingleThreadPool** – cannot be reconfigured; tasks are guaranteed to run sequentially, only one task can run at the same time. **LinkedBlockingQueue** (unbounded) holds the tasks submitted to the executor until they can be executed by the single thread.

**FixedThreadPool** – maintains a fixed number of threads. Works until the explicit shutdown. Creates new thread if one of existing dies. **LinkedBlockingQueue** (unbounded) holds the tasks that can not be executed immediately.

**CachedThreadPool** – has no task queue; to execute new tasks creates new threads that die automatically after 60 seconds of staying idle if they don't pick another task during that time. Suitable for executing many short-lived asynchronous tasks. **SynchronousQueue** used under the hood is a direct handoff queue with no capacity. Each insert operation must wait for a corresponding remove operation by another thread, and vice versa.

**ScheduledThreadPool** – scheduled wrappers of pools above. This thread pool is able to perform a task execution after a given delay, or run the task periodically. **DelayedWorkQueue** is used to delay the execution of tasks.

**SingleThreadScheduledExecutor** – the same as above but optimized for a single thread usage.

**WorkStealingPool** – thread pool that uses a work-stealing algorithm to balance the load among threads. Uses **ForkJoinPool**'s internal queue (a ForkJoinTask array-based Deque). Default parallelism level is processor cores amount. ^fjp

**UnconfigurableExecutorService** – wrapper that forbids reconfiguration.

**Custom ThreadPoolExecutor** – user can create a custom Executor by specifying the core maximum pool size, keep-alive time, and type of a queue. Queue Options:
- **ArrayBlockingQueue**: A bounded blocking queue backed by an array.
- **LinkedBlockingQueue**: An optionally-bounded blocking queue backed by linked nodes.
- **SynchronousQueue**: each insert operation must wait for a corresponding remove operation.
- **PriorityBlockingQueue**: An unbounded blocking queue based on priority of elements.