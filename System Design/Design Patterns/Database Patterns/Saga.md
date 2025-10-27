When each service has its own database, and business transactions involve multiple services, how do we ensure data consistency across services? In this case each request has a compensating request that is executed when the request fails. SAGA can be implemented in 2 ways:

- **Choreography** — When there is no central coordination, each service produces and listens to another service’s events and decides if an action should be taken or not. Choreography is a way of specifying how two or more parties, none of which has any control over the other parties’ processes, or perhaps any visibility of those processes, can coordinate their activities and processes to share information and value. Use choreography when coordination across domains of control/visibility is required. You can think of choreography, in a simple scenario, as like a network protocol. It dictates acceptable patterns of requests and responses between parties.
- **Orchestration** — An orchestrator (object) takes responsibility for a saga’s decision making and sequencing business logic. when you have control over all the actors in a process. when they’re all in one domain of control and you can control the flow of activities. This is, of course, most often when you’re specifying a business process that will be enacted inside one organization that you have control over.

| Choreography               | Orchestration              |
| -------------------------- | -------------------------- |
| ![[design_patterns_3.png]] | ![[design_patterns_4.png]] |

### See Other
- [[System Design/Architecture Patterns/SAGA|SAGA]]

