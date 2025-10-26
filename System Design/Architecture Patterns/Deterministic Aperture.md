This approach allows us to balance requests among servers on a client side, and it perfectly suits [[gRPC]] load balancing. For this solution, we represent clients and servers in a ring with equally distanced intervals, where each object on the ring represents the node or server labeled with a number. This approach is similar to consistent hashing, except that here we guarantee equal distribution of servers across the ring. We’ve also defined an aperture size of 3. Each client selects a starting server with any convenient algorithm like [[Consistent Hashing and HRW]], or just receives it from a configuration server that already knows better choice; yet it creates sessions with 3 next servers clockwise in the ring.

Now we apply continuous ring coordinates that allow us to derive relationships between clients and servers using overlapping ring slices, rather than specific points on the rings. The important thing here is that the overlapping of rings can be partial.

The client picks points (coordinations) in its range instead of picking the instance of the server. The selection process of clients to create sessions with backend servers are as follows:
- Pick two coordinates (offset, offset + width) within its range and map these coordinates to the discrete instances (servers).
- Select the one instance with the most negligible load from two chosen instances (P2C) by its degree of intersection.

**Power of 2 Choices**
The P2C technique for request distribution gives uniform request distribution as long as sessions are uniformly distributed. In P2C, the system randomly picks two unique instances (servers) against each request, and selects the one with the least amount of load. P2C is based on the simple idea that comparison between two randomly selected nodes provides load distribution that is exponentially better than random selection.

[source](https://www.educative.io/courses/grokking-modern-system-design-interview-for-engineers-managers/7AZoPXoP99w)
