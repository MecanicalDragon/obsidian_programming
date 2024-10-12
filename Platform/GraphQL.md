**GraphQL** is a query language and an alternative to [[REST]]. It has its own features comparing to the REST approach, like below:
- GQL can retrieve all the necessary data in a single request, whereas the REST approach must do multiple requests to retrieve all the necessary data.
- GQL uses its own abstractions in work: a *schema* that describes stored data, *queries* to request it, *resolvers* to retrieve it, and *mutations* to update it.
- GQL doesn’t use URLs to retrieve the data but its own specific query language, and requires additional software to build and interpret it both on client and server sides.
- GQL uses POST to convey requests, and it is difficult to cache, so it requires additional efforts to implement caching.
- GQL is risky because it can bring unexpected and unpredictable load onto the database with new updates of clients.
