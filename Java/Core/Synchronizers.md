[Synchronizers](https://m.habr.com/ru/post/277669/) – utils for threads synchronizing that provide more high-level abstractions for this than primitives.<br>
**Semaphore**
<br>Limits access to resource. In constructor semaphore accepts an int that represents a number of threads, that can use the resource. When a thread enters a fenced code block, int decreases, when a thread exits a fenced code block, int increases. Another thread cannot enter, while the int is zero.<br>
**CountDownLatch**
<br>Forbids threads to execute a fenced code until counter (int value, passed in constructor) becomes zero. Any thread can decrease counter. After it becomes zero, all blocked threads become free to proceed. Latch state cannot be reset, it’s disposable.<br>
**CyclicBarrier**
<br>Barrier is a reusable CountDownLatch that also can accept a Runnable as a second parameter. That runnable is executed after the number of awaiting threads reaches the specified value, but before the moment any of awaiting threads proceed their execution. After that barrier resets its state and can be reused.<br>
**Exchanger\<V\>**
<br>Exchanger is a synchronization point between two threads. When thread calls Exchanger’s *exchange()* method, it goes into the awaiting mode. Another thread that calls this method awakes it. Also, these threads exchange that parametrized *V* between them.<br>
**Phaser**
<br>Phaser is an upgraded CyclicBarrier. It’s also a synchronization point for threads-participants, but in phaser participants are not forced to wait for other participants to proceed – they can, but not necessarily, this is for them to decide. Also, participants can register and unregister their participation. Threads, that don’t sign up in participation anyway can track state of the phaser, likewise they can wait for condition meeting, being unsigned. Also, phaser counts phases. Phase ends, when the condition is met; then another phase begins; yet phase counter increases.<br>
[Some additional info](https://habr.com/ru/post/187854/)
