**Partitioning** is a term used to describe the act of breaking up your entities into multiple smaller entities by their logical domains for the purpose of performance, availability, or maintainability.

**Horizontal partitioning** — the act of splitting up the data by some sign. Here could be 2 strategies:
- **Partitioning by key-range** divides partitions based on certain ranges ( A-C, D-F, G-K, L-P, Q-Z). Ranges are not necessarily evenly spaced. Aim is to allocate the same amount of data to different ranges. One of the biggest benefits of such partitioning is the range queries (R-S entries, for example).
- **Partitioning by key hash** uses a hash function to define the correct partition. Hash function can be compiled in a way that allows to avoid hotspots, intrinsic for key-range partitioning. Another advantage – easy to redelegate data with [consistent hashing](https://medium.com/@sandeep4.verma/consistent-hashing-8eea3fb4a598). The flaw of this approach - easiness of range queries disappears.
- **Access pattern** is another example where key partitioning can be employed. Imagine a news-board that is queried for news: a lot of queries for today's news, less for last week, even less for last month. If we use date as a key, we can divide data to equal parts by query number and read news within a day or a week in a single query as everything dwells in the same partition.

**Vertical partitioning** — the act of splitting up the data stored in one entity into multiple entities: `{A, B, C} -> {A, B}, {A, C}`. It can be useful in following cases:
- One part of the data is requested more frequently than another.
- One part of the data is slow-moving (product name, description) and can be cached.
- Sensitive data can be stored in a separate partition with additional security controls.
- We need to reduce the number of concurrent requests, sharing it between two divided tables.

**Functional partitioning** — the act of splitting up the data by its business domain. When it's possible to identify a bounded context for each distinct business area in the application, functional partitioning is a way to improve isolation and data access performance. Another common use for functional partitioning is to separate read-write data from read-only data. This partitioning strategy can help reduce data access contention across different parts of a system.

[Partitioning by Oracle](https://docs.oracle.com/cd/B28359_01/server.111/b32024/partition.htm)
