**Load Balancer Types:**
- **Application (L7) LB** – uses HTTP headers, content type and SSL sessions to provide LB.
- **Network (L4) LB** – uses IP and PORT info extracted from TCP stream to direct the traffic.
- **Global Server (Multi-Site) Load Balancer (GSLB)** - extends the capabilities of L4 and L7 across various data centers. In addition to the traffic balancing, multi-site load balancers also decrease response time for users and mitigate consequences of data center disaster. [[DNS Lookup]] can be deemed as a GSLB load balancer in a way.

Referring to the “*Layer 4 load balancing*” of Internet traffic is a convenient shorthand, but the more accurate term is “Layer 3/4 load balancing” – because the load balancer bases its decision on both the IP addresses of the origin and destination servers (Layer 3) and the TCP port number of the applications (Layer 4). The more exact term for “Layer 7 load balancing” might be “Layers 5-7 load balancing” because HTTP combines the functions of OSI Layers 5, 6, and 7.

L4 LB make their routing decisions based on address information extracted from the first few packets in the TCP stream, and do not inspect packet content. A Layer 4 load balancer is often a dedicated hardware device supplied by a vendor and runs proprietary load-balancing software, and the NAT operations might be performed by specialized chips rather than in software.

When the L4 LB receives a request, it performs Network Address Translation (NAT) on the request packet, changing the destination IP address in the packet from its own to that of the content server it has chosen on the internal network. Similarly, before forwarding server responses to clients, the load balancer changes the source address recorded in the packet header from the server’s IP address to its own. (The destination and source TCP port numbers recorded in the packets are sometimes also changed in a similar way.)

L4 LB was a popular architectural approach to traffic handling when hardware was not as powerful as it is now, and the interaction between clients and application servers was much less complex. L4 LB requires less computation than more sophisticated L7 LB methods, but CPU and memory are now sufficiently faster and cheaper than before, and the performance advantage for L4 LB has become negligible or irrelevant in most situations.

L7 LB operates at the application layer. These LB base their routing decisions on HTTP headers and the actual contents of the message, such as the URL, the type of data (text, video, graphics), or information in cookies. Taking into consideration much more aspects of the transferred information L7 LB becomes more expensive than L4 LB in terms of time and required computing power, but it can nevertheless lead to greater overall efficiency. For instance, because a L7 LB can determine what type of data (video, text, and so on) a client is requesting, you don’t have to duplicate the same data on every inner server. Modern hardware is generally powerful enough that the savings in computational cost from L4 LB are not large enough to outweigh the benefits of greater flexibility and efficiency from L7 LB.

**LB Strategies**:
- **Round Robin** – request is transferred to the first available server and then that server is placed at the bottom of the line.
- **Round Robin + weight** – each server has its own weight to be more or less frequently picked.
- **Least Connections** – traffic is directed to the server having the least traffic.
- **Least Connections + weight** – each server has its own weight for more/less frequent picking.
- **Least Response Time** – least connections + considers the server having the least response time as its top priority.
- **Fixed weighting till failure** – it’s useful if we have an idle reserve server for requests.
- **Least Bandwidth** – traffic is measured in Mbps, and the client request is sent to the server with the least Mbps of traffic.
- **Resource-Based** – assesses servers’ resources availability to direct the traffic.
- **IP Hashing** – assigns the client’s IP address to a fixed server for optimal performance.
- **URL Hashing** – distributes writes uniformly across multiple sites; directs reads to one site that owns a particular object.
- **Source IP Hashing** – client’s and server’s IP addresses are mixed to generate a unique routing hash key.
- **Custom Hashing** – distributes requests based on data in incoming messages (headers, URL, domain name).
- **Location-based**.
- **Sticky Session**.

[Additional Info](https://www.appviewx.com/education-center/load-balancer-and-types/)
