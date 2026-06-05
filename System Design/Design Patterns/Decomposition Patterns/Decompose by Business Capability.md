Microservices are all about making services loosely coupled applying the *single responsibility principle*. This pattern prescribes to decompose applications to services by business capability according to the [[Components Coupling Principles#CCP|CCP Principle]]. A business capability is a concept from [business architecture modeling](https://microservices.io/patterns/decomposition/decompose-by-business-capability.html). It is something that a business does in order to generate value. A business capability often corresponds to a business object, e.g.:
- Order Management is responsible for orders
- Customer Management is responsible for customers
- Logistic management is responsible for logistics

This pattern has the following benefits:
- Stable architecture since the business capabilities are relatively stable
- Development teams are cross-functional, autonomous, and organized around delivering business value rather than technical features
- Services are cohesive and loosely coupled
