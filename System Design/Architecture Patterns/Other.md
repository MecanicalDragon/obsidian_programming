# Ambassador

It is a technique applied in [[Service Mesh]]: each server has its own proxy that serves all server’s needs like encryption/decryption, routing, proxying etc.

# Leader Election

This pattern implies that among all nodes only one takes responsibility to do a certain job. Leader Election is synchronized by special Configuration tools like Zookeeper. With this pattern we can achieve consistent decision making across the distributed system.

# Pub-Sub

Publishers emit events without any care who will receive it. Subscribers receive events without a knowledge of who emitted it. Benefits are decoupling of components, better scaling and performance. PubSub pattern allows to reduce a load on a component and do a job asynchronously.

# Striped Lock

[Striped Lock](https://www.baeldung.com/java-lock-stripping)

# Jitter

[Jitter](https://aws.amazon.com/ru/blogs/architecture/exponential-backoff-and-jitter/) (and Exponential backoff)

# Circuit Breaker

[Circuit breaker](https://docs.microsoft.com/en-us/azure/architecture/patterns/circuit-breaker)

