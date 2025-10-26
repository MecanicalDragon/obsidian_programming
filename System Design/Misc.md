**Fail-safe client-server architecture** – to achieve it we need a leader server (master), several follower servers (slaves) and a witness that pings all servers. If the leader fails, one of the followers takes its place and becomes a new leader. A witness server is needed to control that we have a sole leader and avoid split-brain. To provide required fail-safe, redundancy should be not less than 2. Redis Sentinel is a witness in the fail-safe Redis architecture.

---
**SSO** - Single Sign-On is a technology of a single entrance point; using it user is able to move among different services without everytime authentication.
- **Enterprise SSO** implies an installation of the user agent to the user workstation; this agent ensures an automatic login-password provisioning to all the services.
- **Web SSO** implies a single authentication service for all other connected to it services.

---
**PMP** - Payment Method Provider (Direct money transaction)
**PSP** - Payment Service Provider (PayPal)

---
**To prevent multiple processing of the same operation** you can generate a UUID for each operation and preserve it in the URL of the operation. This allows operation UUID to survive page refreshes, and on the backend prevents multiple processing of a single operation.

---
**3 ways to implement supporting processes for application:**

- **Library** – the cheapest and the fastest by performance approach with all cons, that are mentioned in further approaches as pros, such as strong coupling with app code and common resources.
- **Side Container Daemon Process** – approach is language agnostic, also it can reduce cost, providing resources isolation, but not scalable.
- **Own host** – approach provides resources isolation, independent scalability, flexibility in a hardware choice and possibility to be used by different services.

---
**3 ways to share dynamically changing configuration in distributed system:**

- Add config file as a resource to the application during deployment, redeploy with every update.
- Store config file to the database and make the app poll it periodically.
- Config Service (Zookeeper), that notifies apps about changes.

---
**4 ways to implement redirect**

- **DNS Redirection**. In a typical DNS resolution, we use a [[DNS]] system to get an IP against a human-readable name. However, the DNS can also return another URI (instead of an IP) to the client. Such a mechanism is called DNS redirect.
- **Anycast** is a routing methodology in which all the edge servers located in multiple locations share the same single IP address. It employs the *Border Gateway Protocol* (BGP) to route clients based on the Internet’s natural network flow. A [[CDN]] provider can use the anycast mechanism so that clients are directed to the nearest proxy servers for content. 
	- **BGP** is a network-level protocol used by Internet edge routers to share routing and reachability information so that every node on the network, even if independent, is aware of the status of their closest network neighbors.
- **Client Multiplexing**. Client multiplexing involves sending a client a list of candidate servers. The client then chooses one server from the list to send the request to. This approach is inefficient because the client lacks the overall information to choose the most suitable server for their request. This may result in sending requests to an already-loaded server and experiencing higher access latency. But this could be smoothed with the *P2C* of the [[Deterministic Aperture]] approach.
- **HTTP Status Code 303** - plain old [[HTTP Status Codes|HTTP]] redirection.

---
