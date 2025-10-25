**[Greedy algorithm](https://ru.wikipedia.org/wiki/%D0%96%D0%B0%D0%B4%D0%BD%D1%8B%D0%B9_%D0%B0%D0%BB%D0%B3%D0%BE%D1%80%D0%B8%D1%82%D0%BC)** is an algorithm that makes locally optimal decisions on every step assuming that final solution will be optimal too. ^greedy

---
**Rabin-Karp Algorithm** is an algorithm that solves a task of finding all specified substrings in a string for `O(n)` runtime complexity. *RKA* computes a hash of a specified substring (sum of chars, for example), then it computes it for all potential incomes in a string. Computing window moves through the array; characters that stay behind it are subtracted from a hash, new characters that come to the window are added to the hash. If hashes match, then we perform a precise comparison. [Rabin-Karp](https://www.geeksforgeeks.org/rabin-karp-algorithm-for-pattern-searching/) ^rka

---
**Topological Sort** of a directed [[graph]] is a way of ordering the list of graph nodes such as if `(a-b)` is an edge in the graph then `a` appears before `b` in the list. If the graph has cycles or is not directed it is impossible. The algorithm is to find all nodes with no incoming edges and add them to the result list. Then remove them from the graph and repeat the same with the next nodes with no income. ^topsort
