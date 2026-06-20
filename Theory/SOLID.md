**SOLID** principles help to design a system that is easy to maintain and extend for a long time.

**Single Responsibility** - a class should have a single reason to change.
**Open-Closed** - a class should be closed for changing but open for extending.
**Liskov Substitution Principle** - subtype should be able to replace its supertype leaving the business logic agnostic of this substitution. After the substitution business logic must work in the same way, don't even noticing the fact of the substitution.
**Interface Segregation** - many specific interfaces are better than a single multifunctional one.
**Dependency Inversion** - components should not depend on each other but should depend on abstractions, as other abstractions should do too.

**Inversion of Control** - an approach that allows decreasing coupling among components in the application. The idea is that the programmer places the code in special points where it can be found by the framework and executed when it’s needed.

**Dependency Injection** - approach to IoC that implies that the component delegates the job of obtaining its dependencies to a specially dedicated mechanism (a framework). The advantage is that business logic knows nothing about infrastructure and stays pure.