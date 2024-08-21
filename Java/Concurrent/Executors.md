**FixedThreadPool** – maintains a fixed number of threads. Exists until explicit shutdown. Creates new thread if one of existing dies.<br>
**SingleThreadPool** – cannot be reconfigured; tasks are guaranteed to run sequentially, only one task can run at the same time.<br>
**CachedThreadPool** – has no task queue and creates new threads to do new tasks, that die automatically after 60 seconds of staying idle.<br>
**ScheduledThreadPool** – scheduled wrappers of pools above.<br>
**WorkStealingPool** – uses ForkJoinPool under the hood. Can spread task among free threads. Default parallelism level is processor cores amount.<br>
**UnconfigurableExecutorService** – wrapper, that forbids reconfiguration.<br>