This is an alternative to the [[Decompose by Business Capability]]. That pattern is prone to appearance of so-called *God Classes* which are not easy to decompose. These classes will be common among multiple services. Define services corresponding to [[X-Driven Development#DDD — Domain Driven Design|Domain-Driven Design]] subdomains to avoid it. DDD refers to the application’s problem space — the business — as the domain. A domain consists of multiple subdomains. Each subdomain corresponds to a different part of the business.

Subdomains can be classified as follows:
- **Core** - key functionality for the business and the most valuable part of the application
- **Supporting** - related to what the business does but not a key functionality. These can be implemented in-house or outsourced
- **Generic** - not specific to the business and are ideally implemented using off the shelf software

The subdomains of an Order management include:
- Product catalog service
- Inventory management services
- Order management services
- Delivery management services

This pattern has the following benefits:
- Stable architecture since the subdomains are relatively stable
- Development teams are cross-functional, autonomous, and organized around delivering business value rather than technical features
- Services are cohesive and loosely coupled

[Related article](https://microservices.io/patterns/decomposition/decompose-by-subdomain.html)
