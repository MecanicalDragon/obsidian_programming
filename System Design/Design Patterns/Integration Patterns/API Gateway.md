When an application is broken down to smaller microservices, there are a few concerns that need to be addressed:

- There are multiple calls for multiple microservices by different channels
- There is a need for handling different types of Protocols
- Different consumers might need a different format of the responses

An API Gateway helps to address many concerns raised by the microservice implementation, not limited to the ones above.

- An API Gateway is the single point of entry for any microservice call.
- It can work as a proxy service to route a request to the concerned microservice.
- It can aggregate the results to send back to the consumer.
- It can create a fine-grained API for each specific type of client.
- It can also convert the protocol request and respond.
- It can also offload the authentication/authorization responsibility of the microservice.
