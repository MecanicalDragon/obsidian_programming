Hash Collision is a situation when 2 entries in a HashMap have the same hash and must be stored in the same bucket together. This situation must be tackled somehow, and there are 2 options:
## Chaining

The idea is to make each cell of the hash table point to a linked list of records that have the same hash function value. Chaining is simple but requires additional memory outside the table.

| Advantages                                                                            | Disadvantages                                                                    |
| ------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| Simple to implement                                                                   | Wastage of Space (Some parts of the hash table are never used)                   |
| Hash table never fills up, we can always add more elements                            | Uses extra space for links                                                       |
| Less sensitive to the hash function or load factors                                   | Search time can become O(n) in the worst case for long chain                     |
| Useful when it is unknown how many and how frequently keys may be inserted or deleted | Cache performance of chaining is not good as keys are stored using a linked list |
## Open Addressing

All elements are stored in the hash table itself. Each table entry contains either a record or null. When searching for an element, we examine the table slots one by one until the desired element is found or it is clear that the element is not in the table. Open Addressing can be done in the following ways:
- **Linear Probing** - we linearly probe for the next slot. For example, the typical gap between two probes is 1. *If slot `hash(x) % SIZE` is full, then we try `(hash(x) + attempt) % SIZE`*. Linear probing has the best cache performance and is easy to compute, but suffers from clustering.
- **Quadratic Probing** - we look for the `i2`‘th slot in the `i`’th iteration. *If slot `hash(x) % SIZE` is full, then we try `(hash(x) + attempt*2) % SIZE`*. Quadratic probing lies between the two in terms of cache performance and clustering.
- **Double Hashing** - we use another hash function `hash2(x)` and look for `i*hash2(x)` slot in the `i`’th rotation. *If slot `hash(x) % SIZE` is full, then we try `(hash(x) + attempt*hash2(x)) % SIZE`*. Double hashing has no clustering, but also has poor cache performance and requires more computation time because two hash functions need to be computed.

| Advantages                                                                  | Disadvantages                                           |
| --------------------------------------------------------------------------- | ------------------------------------------------------- |
| Useful when the frequency and number of keys is known                       | Requires more computation                               |
| No links in Open addressing, hence space save                               | Hash table may become full                              |
| Slot can be used even if an input doesn’t map to it                         | Requires extra care to avoid clustering and load factor |
| Provides better cache performance as everything is stored in the same table |                                                         |
## Comparison

| Performance of **`chaining`** hashing can be evaluated under the assumption that each key is equally likely to be hashed to any slot of table                                                                                                                                                   | Performance of `open addressing` hashing can be evaluated under the assumption that each key is equally likely to be hashed to any slot of the table                                                                                                        |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `m` - Number of slots in hash table<br>`n` - Number of keys to be inserted<br><br>Load factor: `α = n/m`<br><br>Expected time to search: `O(1 + α)`<br>Expected time to delete: `O(1 + α)`<br>Time to insert: `O(1)`<br><br>Time complexity of search insert delete:<br>`O(1)` if `α` is `O(1)` | `m` - Number of slots in the hash table<br>`n` - Number of keys to be inserted in the table<br><br>Load factor: `α = n/m (< 1)`<br><br>Expected time to search/insert/delete:<br>`(< 1)/(1 - α)`<br><br>So Search, Insert and Delete take:<br>`(1/(1 - α))` |
