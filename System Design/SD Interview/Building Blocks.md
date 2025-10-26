**Building Blocks** are common puzzle pieces, every distributed system consists of. Each of them aggregates some commonly used functionality and represents a cog in the target system, allowing us to omit the design of features that are already known and widely used. These building blocks are enumerated below:

- **Domain Name System**: This building block focuses on how to design hierarchical and distributed naming systems for computers connected to the Internet via different Internet protocols.
	- **DNS Redirect** is a mechanism that allows returning another URI instead of an IP to the client to perform a routing based on specified determined parts of the URL.

- **Load Balancers**: Load balancer is used to fairly distribute incoming clients’ requests among a pool of available servers. It also reduces load and can bypass failed servers.

- **Databases**: This building block enables us to store, retrieve, modify, and delete data in connection with different data-processing procedures. It includes replication, partitioning, and analysis of distributed databases.
	- **Cold Storage** - approach when seldom-requested data is moved to another storage to reduce a load on the main storage. It perfectly works with an [[aggregate pattern]].

- **Key-Value Store**: It is a non-relational database that stores data in the form of a key-value pair. Important concepts of it are scalability, durability, and configurability.

- **Content Delivery Network**: [[CDN]] is used to keep viral content such as videos, images, audio, and webpages. It efficiently delivers content to end users while reducing latency and burden on the data centers. Treeified proxy structure of CDNs allows them to be more resilient. 
	- CDN may use a push or pull content delivery model, or both. The *PUSH* system is good for static content. *PULL* system suits well for dynamic content.
	- Usually CDN uses an extended treeified proxy structure that allows them to be more resilient and reduce load of core service due to multiple caching layers.
	- A *Point of Presence* (PoP) is a physical place that allows two or more networks or devices to communicate with each other. Typically, each CDN PoP has a large number of cache servers.

- **Sequencer**: A unique IDs generator with a major focus on maintaining causality. Usually, there are five different methods for generating unique IDs: sequence, random generation, timestamps and compounding.
	- *Snowflake* is a unique id generation approach developed in Twitter. It supposes 64-bit ids that follow the following patternю First bit is always zero to make sure that all systems will recognize id as a positive integer. 41 bits for a timestamp with a ms precision. 10 bits for a worker id. 12 bits for a worker sequence. This allows us to maintain a global order of events.
	- *Google TrueTime API* can be used to generate absolutely unique and ordered ids, but it is expensive.

- **Service Monitoring**: Monitoring systems are critical in distributed systems because they help analyze the system and alert the stakeholders if a problem occurs. Monitoring is often useful to get early warning systems so that system administrators can act ahead of an impending problem becoming a huge issue. Nice habit to have two monitoring systems, one for the server-side and the other for client-side errors.

- **Distributed Caching**: Here multiple cache servers coordinate to store frequently accessed data.

- **Distributed Messaging Queue**: This building block is a distributed queue consisting of multiple servers, which is used between interacting entities called producers and consumers. It helps decouple producers and consumers, results in independent scalability, and enhances reliability.

- **Publish-Subscribe System**: An asynchronous service-to-service communication method called a pub-sub system. It is popular in serverless, microservices architectures and data processing systems.

- **Rate Limiter:** A system that throttles incoming requests for a service based on the predefined limit. It is generally used as a defensive layer for services to avoid their excessive usage-whether intended or unintended. See here: [[Rate Limiting]].

- **Blob Store**: This building block focuses on a storage solution for unstructured data—for example, multimedia files and binary executables.
	- *HDFS* - Hadoop Distributed File System - used to store large amounts of files and provide access to it. *BigQuery* or *Amazon S3* are the alternatives.

- **Distributed Search**: A search system takes a query from a user and returns relevant content in a few seconds or less. This building block focuses on the three integral components: crawl, index, and search.

- **Distributed Logging**: Logging is an I/O intensive operation that is time-consuming and slow. This system allows services in a distributed system to log their events efficiently. The system should be scalable and reliable.
	- *Correlation ID* is a system-unique identifier in a multi-component system that allows to track every single request among a number of services. Common-endorsed tag for correlation id is `X-Correlation-ID`.

- **Distributed Task Scheduling**: System mediates between tasks and resources. It intelligently allocates resources to tasks to meet task-level and system-level goals. It’s often used to offload background processing to be completed asynchronously.

- **Sharded Counters**: This building block demonstrates an efficient distributed counting system to deal with millions of concurrent read/write requests, such as likes on a celebrity’s tweet.
