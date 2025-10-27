When breaking the business functionality into several smaller logical pieces of code, it becomes necessary to think about how to aggregate the data returned by each service. This responsibility cannot be left with the consumer. The Aggregator pattern helps to handle this. It talks about how we can aggregate the data from different services and then send the final response to the consumer. This can be done in [two ways](https://dzone.com/articles/design-patterns-for-microservices):
- A composite microservice requests all required microservices, consolidates the data, and transforms it before sending it back.
- An API Gateway can also partition requests to multiple microservices and aggregate the data before sending it to the consumer.

It is recommended if any business logic is to be applied, then choose a composite microservice. Otherwise, the API Gateway is the established solution.

### See other
- [[Aggregate Pattern]]
