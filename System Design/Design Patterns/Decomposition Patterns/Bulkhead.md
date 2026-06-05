Isolate elements of an application into pools so that if one fails, the others continue functioning. This pattern is called in the name of the sectioned ship’s hull; it prescribes to divide service instances into different groups based on service functions, consumer loads and availability requirements. This design helps to isolate failures and allows to sustain service functionality even if some parts of the system are down, preventing cascade failing. This means provisioning of *different Thread pools for different jobs* or *Connection pools for calls to different outer services*. This also means handling of *Kafka messages from different topics in different consumers* for the same reason.

If the bulkheads are structured around *connection pools* that call individual services and Service A fails or causes some other issue, the connection pool is isolated, so only workloads using the thread pool assigned to Service A are affected. Workloads that use Service B, C and others aren't affected and can continue working without interruption.

You can also arrange interservice communication in a way that to each client a separate service instance is assigned. If client 1 made too many requests and overwhelmed its instance, because each service instance is isolated from the others, the other clients can continue making calls.

Use this pattern to:
- Isolate resources used to consume a set of backend services, especially if the application can provide some level of functionality even when one of the services isn't responding.
- Isolate critical consumers from standard consumers.
- Protect the application from cascading failures.

Projects like [resilience4j](https://resilience4j.readme.io/docs/getting-started) and [Polly](https://www.pollydocs.org/) offer a framework for creating consumer bulkheads.
