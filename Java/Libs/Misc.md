Lombok’s `@Data` computes *hashCode* using all fields of the class, hence there may be problems with JPA entities that don’t obtain its id yet or have deep or looped hierarchy.

---
`@Mock` stubs return default primitive values, nulls, or empty collections if you don’t override their behavior.

`@Spy` stubs really invoke stubbed methods if you don’t override them.

---
**JDK Proxy** uses interfaces to proxy a bean, it won’t work without them, yet proxy bean can proxy only denoted in interface public methods. **CGLIB** extends specified class, so if extended class is final, it will fail, on the other hand it doesn’t need interfaces and can proxy protected and package-private methods.

---
**WebClient**’s number of connections affects latency: the higher this number, the higher latency, because it requires more time to search through all these connections to acquire one. On the other hand, a high number of connections allows us to hold more concurrent connections. If all connections are busy, there exists a queue of requests, pending for connection.
