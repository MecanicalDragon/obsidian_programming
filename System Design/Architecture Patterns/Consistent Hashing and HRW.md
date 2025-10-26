### Distributed Hashing

**Distributed Hashing** is plain old hashing that’s used in еру hash map: `server = hash(key) modulo N` where N is the number of servers. Problem here is if a single server goes down or appears the vast majority of all entries must be redistributed. The hint how to mitigate it: use a power of 2 as a number of nodes and double it when you need to increase a node number. This affords to redistribute only a half of the entries.

Time complexities for N nodes and k keys:
- to add a or remove a node - `O(k)`
- to add or remove key - `O(1)`

### HRW

**Highest Random Weight hashing** (also known as **rendezvous hashing** or **HRW**). You hash the key and the server node together (or use the node ID as a hash seed for key hash computation) and then pick the node with the highest hash value. This way the client and provider mutually agree on the meeting location and meet at a selected server. In the case of a uniform hash function, if the node number decreases, k/N entries get spread out over all other nodes instead of just one. If increases – k/N entries will be gathered into a new one from the rest nodes. The biggest drawback of rendezvous hashing is that it runs in O(n) for entry actions. However, because you don’t typically have to break each node into multiple virtual nodes, n is typically small enough for the run-time to be a significant factor. HRW also supports weighting: in the formula `w/h(e)` - `w` is weight and `h(e)` is the hash of entry for this node normalized to `0 - 1`.

Time complexities for N nodes and k keys:
- to add a or remove a node - `O(k/N + log N)`
- to add or remove key - `O(N)`

**HRW Skeleton-based variant**. When N is extremely large, a skeleton-based variant can improve running time. This approach creates a virtual hierarchical structure (called a "skeleton") and achieves O(log n) running time by applying HRW at each level while descending the hierarchy.

### Consistent Hashing

**Consistent Hashing** is a special kind of hashing, which implies that each server node, like all keys to add, has its own hash value. All possible hash values compose a *hash ring*, and all server nodes are represented as points on that ring. When we add a new entry, we compute its hash and find its place on the hash ring, then we move clockwise (or counterclockwise) by that ring, and the first node value we meet will be the storage of this entry (programmatically we can do it with a binary search algorithm). So, if we remove or add one node, we need only rehash values that reside between this node and its adjacent one in counterclockwise (or clockwise) direction. To achieve more even distribution or distribute load among nodes we can add personal *weight* to each node. According to its weight each node will have additional points in different places of the hash ring, so if we move along it we’ll meet pointers for all nodes alternated. So, if we add or remove a server using this scheme, we only need to rehash and remap k/N keys when k is the number of keys and N is the number of servers.

Time complexities for N nodes and k keys:
- to add a or remove a node - `O(k/N + log N)`
- to add or remove key - `O(log N)`

Consistent Hashing can be considered as a special case of HRW if we talk about the task of finding k most appropriate sites. Clients can independently pick sites with the k's largest hash values because they compute hashes for all sites anyway.

For HRW we only need a list of server identifiers to locate the server that manages a given entry, since we can locally compute all hash function values. Consistent hashing has a state and requires more memory to store all its virtual nodes, but it does less computation while computing a node for an entry.

For both *CH* and *HRW* in a distributed system it’s always a problem to add new nodes because a new node becomes preferable for k/N arbitrary entries in the system, so we must verify that all of the entries in the system are managed by the correct node. For caching systems it’s not a problem, because over time newly assigned values will be fetched automatically.
