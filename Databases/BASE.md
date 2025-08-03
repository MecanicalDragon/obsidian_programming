**BASE** is an [[ACID]]-alternative approach to data storing: *Basically Available, Soft-state, and Eventually consistent*.

**Basically Available** means that failure in several nodes leads to service failure only for the insignificant part of sessions, saving availability for the most sessions to live on. The system does guarantee availability in [[CAP]] terms.

**Soft-state** stands for the possibility of the stored data not being consistent at the moment of time or different replicas not being mutually consistent.

**Eventual consistency** allows data contradiction in some cases, right after the update, but in the end, in practically reachable time all data will become consistent.

Contrary to SQL databases, [[NoSQL databases]] were designed with scale in mind. Therefore, the scaling process is largely invisible to the end-user as it is done automatically. NoSQL also has shards of databases that correspond to different indices. However, one key difference is that instead of a master-slave architecture, every shard is equal. Nodes are able to communicate and exchange information about each other — they know where a particular data is stored if the data is not contained within itself.

When a query comes in, an initial coordinator node within the cluster is picked to process the request via the chosen algorithm. Such algorithms include round-robin, or shortest distance to the node). If the coordinator node does not own the data, it forwards the request to another node who has the data or knows another node who has it.

Similarly, when a write request goes to a coordinator node, it uses [[consistent hashing]] to determine which node should the data be stored in. The whole idea of sharing information through the nodes is often referred to as the *gossip protocol*, where information is transferred from node to node like an epidemic.

Consistency is achieved by replicating the data from node to node. That happens asynchronously and takes time. Therefore, instead of waiting for the response from all nodes, coordinator node commits write when it receives a specific number of acknowledgements from other nodes. This approach is called *quorum writes*.

Availability in NoSQL paradigm says that it’s preferred to show stale data than no data at all. After all, there will be eventual consistency amongst the nodes, as explained in the *gossip protocol* earlier. When a read query comes to the coordinator node, which then initiates parallel read requests to the other nodes, it’s possible that different responses will be returned. Just as we have *quorum writes*, we also have *quorum reads*, which means the response that x number of nodes agree on is returned.
