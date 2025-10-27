When microservices come into the picture, we need to address a few issues in terms of calling services. With container technology, IP addresses are dynamically allocated to the service instances. Every time the address changes, a consumer service can break and need manual changes. Each service URL must be remembered by the consumer and become tightly coupled.

A [service registry](https://www.dineshonjava.com/microservices-with-spring-boot/) needs to be created which will keep the metadata of each producer service and specification for each. A service instance should register to the registry when starting and should deregister when shutting down. There are two types of service discovery: client-side, like Netflix Eureka, or server-side, like AWS ALB.

![[design_patterns_5.png]]
