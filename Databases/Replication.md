**Replication** means data copying among servers. Usually exist Master, used for writing data, and Slaves for copying data from master and providing reads. Main advantage – a lot of data copies and possibility to replace master on-flight in a fault case. But it has a disadvantage too: delay between data write and availability of this data for reading. Also exists *Master-Master* replication, but it is considered as a bad approach, because it’s slow in performance and easily leads to data inconsistency.

[Single Leader Replication](https://towardsdatascience.com/database-replication-explained-5c76a200d8f3)

**What types of replications exist?**
- **Sync replication** – guarantees data consistency but increases latency.
- **Async replication** – provides low latency but can hurt consistency in case of network failures.
- **Sync-Async replication** – looks like a good solution.

**Where to read data from?**
- **Leader** – hurts performance.
- **Follower** - if async replication is picked, a client can retrieve stale data. Solutions:
	- Use timestamps for read-after-write consistency (the client always knows the timestamp of the latest update, yet only values younger than that timestamp can be returned by the database).
	- Sticky routing for monotonic read consistency (client always reads data from one follower to prevent different (inconsistent) data read).

**What if the leader fails?**
- If async replication is used, data may be lost.
- Another Leader will be picked, but if the previous leader resurrects, data inconsistency arises.
- If a network is partitioned into two subsets due to network problems, *Split-Brain* situation arises, and the subset with the majority of nodes remains active while shooting the minority subset (*STONITH* — Shoot The Other Node In The Head) by sending out a special signal to power supply controller.

[Multi-Leader Replication](https://towardsdatascience.com/database-replication-explained-10ff929bdf8a)

Provides better scalability and availability, but concurrent updates may lead to data collisions. Solutions:
- **Assign timestamps to write requests**. In case of collision, the latest wins. Moreover, even sequential updates may lead to collision, if the clock of the single servers is skewed. Anyway, if we use this solution, it inevitably leads to data loss for updates that have not won this race.
- **Fixed routing**. All write requests to the same entry route to the same leader. Simply. This solution is used in *Redis Cluster*.
- **Custom algorithm**. Server doesn’t resolve collisions. It stores all values and sends them to the client when requested. Client knows how to resolve collisions, does it and sends an update to the server.

**Topologies**
- **Broadcasting**. Each leader sends updates to all other `n` leaders for `O(n^2)`
- **Ring topology**. Each leader only sends messages to another leader. This algorithm takes only `O(n)`, but if any node failure happens, the entire communication link is severed, which makes it less robust.
- **Star topology**. Implies one central node, that serves all updates and becomes a single point of failure, that makes this topology fragile, but any communication between two nodes can be delivered with at most two hops.

[Leaderless Replication](https://towardsdatascience.com/database-replication-explained-3-32d6deceeca7)

This approach is about *gossip protocol*, *quorum reads* and *quorum writes*. Leaderless replication is highly available because all nodes accept reads/writes. Number of nodes that must acknowledge a request to consider it as completed is configurable.

**Strict Quorum** is a type of configuration that requires `write quorum + read quorum > Total node number`. This config guarantees strong read consistency (but not 100%). Anyway, it is possible that the client can be cut off from many nodes and unable to reach quorums. In this case separated nodes can be configured to be allowed to accept writes, yet they must send updates to home nodes when they become available. This configuration type is known as a **Sloppy Quorum**.

For read requests in network partition situations we can either enforce *strict quorum* (read-only from home nodes), which offers better consistency, or, if higher availability is needed, enable *sloppy quorum* as well, that may lead to stale reads. For systems with a balanced read/write ratio it is sensible to set `Writes` and `Reads` to `Nodes/2 + 1` for good performance.

**Problems**:
- Strong consistency (linearizability) requires absence of version-flipping (read twice, first get new value and then stale value). Even a strict quorum cannot guarantee linearizability when the network is unstable.
- Failure handling may lead to inconsistency too. If `ACK` from any node is delayed, the write request is considered as failed, but another client is still able to see the dirty value. This problem is inherent to leaderless architecture since individual nodes have no global information about the status of request. Distributed transactions can handle this problem with a cost significant system delay increase.
