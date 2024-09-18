Java offers two models of asynchronous programming:

- **Callbacks** - asynchronous methods don’t return value but take an extra callback parameter (anonymous class or lambda), which is called when the result is available. A well-known example is Swing’s EventListener hierarchy.
- **Futures** - asynchronous methods immediately return a `Future<T>`. The asynchronous process computes a `T` value, but the Future object wraps access to it. The value is not available immediately, and the object cannot be polled until the value is available. For instance, an ExecutorService running `Callable<T>` tasks use Future objects.

## Hot and Cold methods

The Rx family of reactive libraries distinguishes two broad categories of reactive sequences: [hot and cold](https://projectreactor.io/docs/core/release/reference/#reactor.hotCold). This distinction determines how the reactive stream reacts to subscribers:
- **A Cold sequence** starts anew for each Subscriber. For example, if the source wraps an HTTP call, a new HTTP request is made for each subscription. If no subscription is created, data never gets generated.
- **A Hot sequence** does not depend on the number of subscribers and does not start from scratch for each Subscriber. It starts publishing data right away from the start and continues doing so whenever a new Subscriber comes in, so late subscribers receive only signals emitted after they subscribed. Note, however, that some hot reactive streams can cache or replay the history of emissions totally or partially. From a general perspective, a hot sequence can even emit when no subscriber is listening (an exception to the “nothing happens before you subscribe” rule).

One example of the few hot operators in Reactor is *just* - it directly captures the value at the assembly time and replays it to anybody subscribing to it later. To transform *just* into a cold publisher, you can use *defer*. It defers wrapped logic till the subscription time and executes it for each new subscription. On the opposite, *share()* and *replay(​)* can be used to turn a cold publisher into a hot one (at least once a first subscription has happened).

## Mono and Flux

`Mono.map()` transforms the item emitted by this Mono by applying a synchronous function to it.
`Mono.flatMap()` transforms the item emitted by this Mono asynchronously, returning the value emitted by another Mono.

## Schedulers

In Reactor, execution model and execution context (where the execution happens) are determined by the *Scheduler* that is used. A Scheduler has scheduling responsibilities similar to an *ExecutorService*. `Schedulers` class provides various Scheduler flavors that can be involved using *publishOn* or *subscribeOn* methods:

- **single**: *Optimized for low-latency Runnable one-off executions*. This scheduler reuses a single thread for all callers, until it is disposed. If you want a per-call dedicated thread, use `Schedulers.newSingle()` for each call.
- **parallel**: *Optimized for fast Runnable non-blocking executions*. This is a fixed pool of workers that is tuned for parallel work. It creates as many workers as CPU cores you have.
- **elastic**: *Optimized for longer executions*, an alternative for blocking tasks where the number of active tasks (and threads) can grow indefinitely. This one is no longer preferred with the introduction of *Schedulers.boundedElastic()*, as it tends to hide back pressure problems and create too many threads.
- **boundedElastic**: *Same as elastic, but the number of active tasks (and threads) is capped*. Like its predecessor *elastic*, it creates new worker pools as needed and reuses idle ones. Worker pools that stay idle for too long (the default is 60s) are also disposed. Unlike elastic, it has a limitation on the number of backing threads it can create (default is number of CPU cores x 10). Up to 100 000 tasks submitted after the cap has been reached are enqueued and will be rescheduled when a thread becomes available. This is a better choice for I/O blocking work. This scheduler is a handy way to give a blocking process its own thread so that it does not tie up other resources.
- **immediate**: *To immediately run the submitted Runnable* instead of scheduling it. This one has no execution context, at processing time, the submitted Runnable will be directly executed, effectively running them on the current Thread (can be considered as a "null object" or no-op Scheduler).
- **fromExecutorService**: *To create new instances around Executors*. You can create a Scheduler out of any pre-existing ExecutorService. You can also create one from an Executor, although doing so is discouraged.

Factories prefixed with new (e.g., *newBoundedElastic()*) return a new instance of their flavor of Scheduler, while other factories like *boundedElastic()* return a shared instance – one which is used by operators requiring that flavor as their default Scheduler. All instances are returned in a started state.

## Publish and Subscribe

Most operators continue working in the Thread on which the previous operator executed. Unless specified, the topmost operator (the source) itself runs on the Thread in which the *subscribe()* call was made.
```java
final Mono<String> mono = Mono.just("hello"); // The `mono` is created in thread `main`.
new Thread(() -> mono
	.map(msg -> msg + " from the thread ")
	.subscribe(m ->  // However, it is subscribed to in thread `Thread-0`
		System.out.println(m + Thread.currentThread().getName())  // Therefore, both `map` and `onNext` callbacks actually run in `Thread-0`
	)
).start();
```

*publishOn()* applies in the same way as any other operator, in the middle of the subscriber chain. It takes signals from upstream and replays them downstream while executing the callback on a worker from the associated Scheduler. Consequently, it determines where the subsequent operators execute (until another *publishOn* is chained in).
```java
Scheduler s = Schedulers.newParallel("parallel-scheduler", 4); // 1. Creates a new Scheduler backed by four Thread instances.
final Flux<String> flux = Flux
	.range(1, 2)
	.map(i -> 10 + i)  // 2. The first `map` will run on the anonymous thread in <5>.
	.publishOn(s)  // 3. The `publishOn` switches the whole sequence tp a Thread picked from <1>.
	.map(i -> "value " + i);  // 4. The second `map` runs on the Thread from <1>.
new Thread(() -> flux.subscribe(System.out::println)).start();  // 5. This anonymous Thread is the one where the subscription happens. The `println` happens in the latest execution context, which is the one from `publishOn`.
```

*subscribeOn()* is applied during the subscription process (when the whole chain is constructed). Consequently, no matter where you place the *subscribeOn* in the chain, it always affects the context of the source emission. However, this does not affect the behavior of subsequent calls of *publishOn* — they still switch the execution context for the part of the chain after them. Only the earliest *subscribeOn* call in the chain actually matters.
```java
Scheduler s = Schedulers.newParallel("parallel-scheduler", 4); // Creates a new Scheduler backed by four Thread instances.
final Flux<String> flux = Flux
	.range(1, 2)
	.map(i -> 10 + i)  // The first `map` runs in one of these four threads…
	.subscribeOn(s)  // …​because `subscribeOn` switches the whole sequence right away since the subscription happened (last line).
	.map(i -> "value " + i);  // The second `map` also runs in the same thread.
new Thread(() -> flux.subscribe(System.out::println)).start();  // This anonymous Thread is the one where the subscription initially happens, but `subscribeOn` immediately shifts it to one of the four scheduler threads.
```

By default, HttpClient uses a “fixed” connection pool with 500 as the maximum number of active channels and 1000 as the maximum number of the pending threads waiting for the channel acquisition. This means implementation creates up to 500 channels and keeps 1000 more requests in a pending queue, [further attempts are declined with an error](https://projectreactor.io/docs/netty/release/reference/index.html#_connection_pool_2).
