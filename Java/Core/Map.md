## HashMap
<br>First of all, hash map has several private parameters:
- **CAPACITY** - initial size of inner bin array, must be a power of 2, so map, being initialized, picks the next to the specified capacity value power of 2 (16 is default).
- **LOAD_FACTOR** = 0.75 by default.
- **THRESHOLD** - value that is calculated during hashmap initialization and on each resize. If map size exceeds **CAPACITY** \* **LOAD_FACTOR**, map doubles the bin array.<br>
**TREEFY_THRESHOLD** = 8; 
**UNTREEFY_THRESHOLD** = 6; 
**MIN_TREEFY_CAPACITY** = 64;
<br>When a new entry is added to the map, map calculates its hash: `(key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16)` and builds a **Node** that contains 4 parameters: this cached hash, key, value and the next node reference. Cached hash means if you use mutable objects for keys and change key later, cached hash won’t change and entry will stay in its place, so you just can’t find it by the key. Map defines bin index for entry computation by formula: `hash & bucket_array_length – 1`, that is equal to hash modulo of current map capacity. Map adds a new node to the computed bucket. If the bucket is not empty, map compares a new node to the nodes that already dwell there one by one, first by hash, then by the *equal()* method and replaces node that is equal to the new one, or chains this new node to the end of bucket’s node list if no match.<br>
If the list of the nodes inside a bin becomes too long and exceeds **TREEFY_THRESHOLD**, *treeifyBin()* method is being called. If the bin array size has not reached **MIN_TREEFY_CAPACITY**, this method resizes the map (doubles bin array and redistributes nodes), and turns the node list into a tree otherwise. Being arranged as a tree, nodes are compared in the following order:<br>
1. Map compares hashes.
2. If hashes are equal, map compares keys by link and equals method.
3. If keys don’t match, map checks if key instances implement Comparable and compares them.
4. If they don’t, the map compares their classnames.
5. If key classnames are equal, map compares keys’ identityHashCodes.
6. If even identity hashcodes are equal, map just puts the value to the left branch.
<br>HashMap rebalances the tree on each insertion or deletion.
## TreeMap

TreeMap is a self-balancing red-black tree that rebalances itself after element insertion or removal and provides **O(log n)** runtime complexity for the most operations.<br>
Map requires either to use comparables as keys or to use a custom comparator. If map’s comparator is null, map compares keys as comparables and throws **ClassCastException** if the key doesn’t implement it. By default, TreeMap cannot handle null keys and throws **NPE**, but this behavior can be overridden in a custom comparator. <br>
When you add an element, it’s been compared with the root element of the tree. If the result is lower than zero, the element goes to the left branch, going to the right one otherwise. All branches end with null leaves, so map returns null in the end if the key is not presented.

| HashMap                                                   | TreeMap                                                |
| --------------------------------------------------------- | ------------------------------------------------------ |
| Doesn’t ensure order of elements                          | Provides natural order of elements                     |
| Accepts null as a key                                     | By default, throws NPE if key is null                  |
| Can be configured to hold the expected number of elements | Doesn’t require additional memory for inner structures |
