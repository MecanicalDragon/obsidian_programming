Redis Cluster is a multi-master distributed database that consists of multiple nodes, each can be a Master or a Replica. Replica tries to be an exact copy of Master and receives a stream of updates from it. In case of connection issues Replica requests for partial or full (if partial fails) resynchronization before the stream resumes. In the last case Master sends a full data snapshot to Replica. By default, Redis uses asynchronous replication, but synchronous replication of certain data can be requested by the clients using the *WAIT* command. However, WAIT only ensures that data was acknowledged in a specified number of replicas; it does not turn a set of Redis instances into a CP system with strong consistency.

Redis Cluster is a full mesh where every node is connected with every other node using a [[TCP]] connection. These connections are not created on demand but kept alive all the time. Nodes use a *gossip protocol* to avoid exchanging too many messages among them. If the master node is unavailable long enough, one of the replicas will be promoted to master, and it may lead to data loss due to async replication. Though Redis is AP, Redis Cluster is not available in the minority side of the partition if it happens.

Redis Cluster is horizontally scalable, HA, fault-tolerant, and shards data automatically. To distribute data Redis Cluster makes use of *hash slots*. There is a total number of 16384 hash slots, and every node in the cluster is responsible for a subset of them. When a request is sent for a given key, a client computes a hash slot for that key and sends it to an appropriate server. It means Redis Cluster client must be cluster-aware: update hash slots periodically and handle redirects themselves. Furthermore, since data can be located on different nodes, clients also should care about the handling of multi-key operations. Common Redis use cases are:
- distributed cache
- user session storage
- distributed locks mechanism
- distributed rate limiting

Why Redis is so fast: it is in-memory and single-threaded, and makes use of [[Tech/Misc#^epoll|epoll]] event loops.

[Redis Topologies: Replicated setup, Sentinel, Cluster, Standalone + Envoy](https://blog.devgenius.io/redis-topologies-d9e16a7fa8e0)

[Измышления на Хабре](https://habr.com/ru/post/322276/)
