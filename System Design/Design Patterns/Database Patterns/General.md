Defining the database architecture for microservices we need to consider the following points.

1. Services must be loosely coupled. They can be developed, deployed, and scaled independently.
2. Business transactions may involve multiple services.
3. Some business transactions need to query data that are owned by multiple services.
4. Databases may be replicated and sharded in order to scale.
5. Different services have different data storage requirements.
