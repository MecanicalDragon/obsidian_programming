Here are the four benefits that *Microservice Architecture* (*MSA*) can bring to enterprise:
1. **Reduce Cost**: MSA reduces overall cost of designing, implementing, maintaining of services.
2. **Increase Release Speed**: MSA increases the speed from idea to deployment of services.
3. **Improve Resilience**: MSA improves the resilience of our service network.
4. **Enable Visibility**: MSA supports for better visibility on your service and network.

MSA has been built with the following principles:
- Scalability
- Availability
- Resiliency
- Flexibility
- Independent, autonomous
- Decentralized governance
- Failure isolation
- Auto-Provisioning
- Continuous delivery through DevOps

Following principles above can bring different challenges and issues that are common for many solutions. To overcome it, MSA design patterns exist. These design patterns can be divided into five groups. The diagram below depicts it.

![[design_patterns_1.png]]

| Microservices Pros                                                                 | Microservices Cons                                                                           |
| ---------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| Allow to employ side specialists to develop separate services in high-load periods | More complicated and expensive maintenance, additional load balancer for each scaled service |
| Easier to understand distinct modules                                              | Higher employees’ qualification requirements                                                 |
| More scalable resources and scheduled jobs                                         | Transactions, testing, calls time overhead                                                   |
| Possibility to scale functionality that needs it                                   | Complicated infrastructure (K8s)                                                             |
|                                                                                    | Complicated invocation flow over the services                                                |
|                                                                                    | Inconsistent database                                                                        |
