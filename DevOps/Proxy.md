**Proxies** can be *Forward Proxies* and *Reverse Proxies*.

**A forward proxy server** is an intermediary server that forwards requests from multiple clients to the Internet. It allows to:
- Hide client’s online identity.
- Bypass browser restrictions.
- Block access to certain content.

**A reverse proxy server** is a type of proxy server that is typically located behind the firewall in a private network and directs incoming requests to the appropriate backed server. A reverse proxy provides an additional level of abstraction and control to ensure the smooth flow of network traffic between clients and servers. Common uses for a reverse proxy server include:
- [[Load balancing]] – A reverse proxy server locates in front of backend servers and distributes client requests across a group of servers in a manner that maximizes speed and capacity utilization.
- **Web acceleration** – Reverse proxies can compress inbound and outbound data, as well as cache commonly requested content, both of which speed up the flow of traffic between clients and servers. They can also perform additional tasks such as [SSL encryption](Network/TLS) to take off a load of backed web servers, thereby boosting their performance.
- **Security and anonymity** – By intercepting requests headed for backend servers, a reverse proxy server protects their identities and acts as an additional defense against security attacks. It also ensures that multiple servers can be accessed from a single record locator or URL regardless of the structure of the local area network.
