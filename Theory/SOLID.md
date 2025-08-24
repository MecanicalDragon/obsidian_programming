**SOLID** principles help to design a system that is easy to maintain and extend for a long time.

**Single Responsibility** - a class should have a sole cause to change.
**Open-Closed** - class should be closed to changes but open to extensions.
**Liskov Substitution Principle** - if a class is replaced by its subtype, program logic must not be affected by this change.
**Interface Segregation** - a plethora of specific interfaces is better than a single multifunctional one.
**Dependency Inversion** - components must depend not on each other but on abstractions, so as other abstractions either.

**Inversion of Control** - an approach that allows decreasing coupling among components in the application. The idea is that the programmer places the code in special points where it can be found by the framework and executed when it’s needed.

**Dependency Injection** - approach to IoC that implies that the component delegates the job of obtaining its dependencies to a specially dedicated mechanism (a framework). The advantage is that business logic knows nothing about infrastructure and stays pure.