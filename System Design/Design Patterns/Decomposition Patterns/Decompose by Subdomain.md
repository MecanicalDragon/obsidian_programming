Decomposing an application using business capabilities might be a good start, but you will come across so-called “God Classes” which will not be easy to decompose. These classes will be common among multiple services. Define services corresponding to Domain-Driven Design (DDD) subdomains to avoid it. DDD refers to the application’s problem space — the business — as the domain. A domain consists of multiple subdomains. Each subdomain corresponds to a different part of the business.

Subdomains can be classified as follows:
- **Core** - key functionality for the business and the most valuable part of the application
- **Supporting** - related to what the business does but not a key functionality. These can be implemented in-house or outsourced
- **Generic** - not specific to the business and are ideally implemented using off the shelf software

The subdomains of an Order management include:
- Product catalog service
- Inventory management services
- Order management services
- Delivery management services
