First of all, they are not explicitly bad, but we must pay attention to create and use it.

Utility class is a support class that defines a set of methods that perform common, frequently reused functions.
- Utility class should not contain business logic, because for business logic we’ve got beans.
- Utility class is a support class, and it must contain support methods.
- Utility class must contain methods related to some idea.

Disadvantages:
- It is not OOP-related, it is procedure-related.
- It doesn’t provide abstraction to communicate, that is one of basic OOP principles.
- Utility classes provoke ugly code because you can stash everything there and do not care about design.
- Utility classes provoke ugly code because you can use it everywhere and do not care about dependencies.
- Utility class is not shown in bean’s dependencies, it causes leaking dependencies.
- It can’t be proxied; it is difficult to mock and test.
- With time passing, utility class tends to collect a lot of methods that are not relative at all; class becomes a rubbish dump of very diverse code.
- A plenty of utility classes make code incomprehensible and tightly coupled, because components obtain a lot of hidden dependencies, and it becomes unclear which components the target component depends on and which logic it executes.

[https://medium.com/@sahoosunilkumar/utils-class-good-and-or-bad-3f0d867ffc39](https://medium.com/@sahoosunilkumar/utils-class-good-and-or-bad-3f0d867ffc39)
[https://www.vojtechruzicka.com/avoid-utility-classes/](https://www.vojtechruzicka.com/avoid-utility-classes/)
