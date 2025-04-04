## N+1 Problem

Let’s imagine we have an `Order` entity that relates in a `1:m` manner to the `Cargo` entity. We want to retrieve the last 10 orders. Default Hibernate settings perform 1 request for orders + 1 request for cargoes for each order. Here we’ve got *n+1 total requests*.

**Possible solutions**

`FetchMode.JOIN` (becomes `JOIN FETCH` in HQL). Bad solution.
- `offset` and `limit` don’t work. If the first order contains 10+ cargoes we’ll retrieve only the first 10 cargoes of this order. This can be mitigated by in-memory handling of offsets and limits, but you understand it sucks.
- *Cartesian Product* Problem. N orders with M cargoes and K dispatchers query returns `N*M*K` entries.

`FetchMode.SUBSELECT`. This mode uses the main query as a subselect query for the dependent entities. In result we’ve got only 2 queries instead of 11. Anyway, it has its own flaws:
- It is specified in the entity's properties (`@Fetch`), so in future someone can change it and revive n+1 problem.
- If the main query is complex, the whole complexity will be applied for subquery too.

`FetchMode.SELECT`+`@BatchSize`. Mode uses distinct query for dependent entities with condition like `WHERE order_id IN (...)`. And the `IN` operator [[SQL IN|has its own specifics]].

*JPA Entity Graph* and *Hibernate FetchProfile* allow us to create abstraction for entity loading rules.

Custom request handling: first, we load main entity collection, then reduce it to ids and load dependent entity collection with `WHERE order_id IN (...)`.
## MultipleBagFetchException

**MultipleBagFetchException** is thrown to indicate that a query is attempting to simultaneously fetch multiple bags.

A **Bag** is a Hibernate’s collection that can contain duplicate elements - just like `java.util.List` but not ordered. However, it implements `java.util.List` and is used when an entity has a `List` of dependent entities that are not arranged in any order (that can be specified with `@OrderColumn`). Fetching two or more bags at the same time could form a *Cartesian Product*. Since a Bag doesn't have an order, Hibernate would not be able to map the right columns to the right entities. Hence, in this case, it throws a `MultipleBagFetchException`. This can happen if we’ve got an entity that has 2 bags and both of them have eager fetch types. To avoid it, we’ve got 3 ways:

1. Convert one or both collections' fetch type to lazy. Anyway, if we try to fetch both of these bag collections at the same time, even using jpql, it will still lead to `MultipleBagFetchException`.
2. Using a single Bag in an entity and `Sets` or `Lists` with an `@OrderColumn` as other collections, we avoid multiple Bag fetching and, hence, `MultipleBagFetchException`. However, since we fetch two and more collections simultaneously in a single query, this approach generates a *Cartesian Product*.
3. The best choice is to use several queries in a single transaction.

[source](https://www.baeldung.com/java-hibernate-multiplebagfetchexception)

## @Transactional nuances

If a method in a service calls another `@Transactional` method of the same service, the transaction won’t be opened, because transaction-proxy won’t be aware of this call. `@Transactional` annotation wraps service into a proxy, and this proxy logic can be triggered only from the outside of the service but not from the inside of the wrapped service.

By default Spring rollbacks a transaction only if a thrown exception is unchecked; checked exceptions commit a transaction. To adjust this behavior parameters `rollbackFor` and `noRollbackFor` of `@Transactional` exist. Flag `readOnly` can be set to true if the transaction is effectively read-only, allowing for corresponding optimizations at runtime.

Let’s imagine that one `@Transactional` service A calls another `@Transactional` service B. Service B throws an unchecked exception, but Service A catches it, handles and returns from the method with no exception thrown. It seems that Service A transaction should be committed (unlike Service B transaction), but it isn’t. That’s because default `@Transactional` propagation is `REQUIRED` that means that the same transaction is used in Service A and Service B. Therefore, since Service B transaction-proxy registered an unchecked exception, it marked the transaction as rollback-only, so changes won’t be committed.

