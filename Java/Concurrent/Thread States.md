**New** – thread is just created, but not yet started.
**Runnable** – thread runs.
**Blocked** – thread waits for object monitor, and only acquiring it can turn it to runnable state.
**Waiting** – *wait*, *join* or *park* method has been called. *notify()* and *notifyAll()* awake the thread again.
**Timed Waiting** – *sleep*, *wait*, *join*, *park* method with timeout has been called. *notify()*, *notifyAll()* or time elapsing awake the thread.
**Terminated** – thread finished its work and cannot be started again.