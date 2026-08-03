**False Sharing** is a situation when different processor cores work with different variables that reside in the same [[Cacheline]]. Since only one core at time can work with any cache line due to consistency purposes, these values can't be modified simultaneously. To avoid this Java has annotation [[Contended]], and processors use the [[MESI]] protocol.

[false sharing](https://alidg.me/blog/2020/4/24/thread-local-random#false-sharing)
