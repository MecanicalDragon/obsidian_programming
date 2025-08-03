## R-Tree

**R-tree** — rectangle-tree, a tree data structure used for spatial access methods, i.e., for indexing multi-dimensional information such as geographical coordinates, rectangles or polygons. The key idea is to group nearby objects and represent them with their [minimum bounding rectangle](https://en.wikipedia.org/wiki/Minimum_bounding_rectangle) in the next higher level of the tree. Since all objects lie within this bounding rectangle, a query that does not intersect the bounding rectangle also cannot intersect any of the contained objects. At the leaf level, each rectangle describes a single object; at higher levels the aggregation includes an increasing number of objects. This can also be seen as an increasingly coarse approximation of the data set.

## K-d tree

**K-Dimensional tree** is a type of binary search tree where each node represents a point in the k-dimensional space. This one is pretty similar to *R-tree*, but for the k-dimensional space. The K-d tree is unbalanced and allows you to search keys in a particular range. K-d tree can be homogeneous and heterogeneous. In the homogeneous one each node keeps an entry, In the heterogeneous one nodes keep only keys; entries are kept in leaves.

## Quadtree

**Quadtree** is a tree data structure in which each internal node has exactly four children. Quadtrees are the two-dimensional analog of octrees (quadtrees for 3-dimensional space, which divide into 8 subnodes) and are most often used to partition a two-dimensional space by recursively subdividing it into four quadrants or regions. Leaf cell represents a "unit of interesting spatial information". All forms of quadtrees share some common features:
- They decompose space into adaptable cells
- Each cell (or bucket) has a maximum capacity. When maximum capacity is reached, the cell splits
- The tree directory follows the spatial decomposition of the quadtree.

A tree-pyramid (**T-pyramid**) is a "complete" tree; every node of the T-pyramid has four child nodes except leaf nodes; all leaves are on the same level, the level that corresponds to individual pixels in the image. The data in a tree-pyramid can be stored compactly in an array as an implicit data structure similar to the way a complete binary tree can be stored compactly in an array.

## Trie

**Trie** is a prefix tree. Nodes in the trie do not store their associated key. Instead, a node's position in the trie defines the key with which it is associated. This distributes the value of each key across the data structure, and means not every node necessarily has a value. All the children of a node have a common prefix of the string associated with that parent node, and the root is associated with the empty string. This task of storing data accessible by its prefix can be accomplished in a memory-optimized way by employing a *radix tree*.

## Radix Tree

**Radix tree** is a data structure that represents a space-optimized trie in which each node that is the only child is merged with its parent. The result is that the number of children of every internal node is at most the radix r of the radix tree, where r is a positive integer and a power x of 2, having x ≥ 1. Unlike regular trees, edges can be labeled with sequences of elements as well as single elements. This makes radix trees much more efficient for small sets (especially if the strings are long) and for sets of strings that share long prefixes.

## Bloom Filter

**A Bloom filter** is a space-efficient probabilistic data structure that is used to test whether an element is a member of a set. False positive matches are possible, but false negatives are not – in other words, a query returns either "*possibly in set*" or "*definitely not in set*". Elements can be added to the set, but not removed (though this can be addressed with the *counting Bloom filter* variant); the more items added, the larger the probability of false positives.
