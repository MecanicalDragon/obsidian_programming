1. **Inflators** (increase code [[Clean Code#4 metrics of code cleanliness|congestion]]):
    - long methods / large classes.
    - [primitive obsession](https://refactoring.guru/ru/smells/primitive-obsession) - primitive types usage instead of dedicated classes (`int finalCallResult` instead of `enum CallResult`).
    - too many method arguments.
    - [data clumps](https://refactoring.guru/ru/smells/data-clumps) - a set of related data variables that always appear together over the codebase and together serve a single purpose.
2. **OO Design Violators** (hit [[Clean Code#4 metrics of code cleanliness|complexity]]):
	- Extensive switch / if operators.
	- Object fields that are used rarely and only under specific circumstances.
	- [Refused bequest](https://refactoring.guru/ru/smells/refused-bequest) ([Replace inheritance with delegate](https://refactoring.guru/ru/replace-inheritance-with-delegation)).
	- Different classes with different interfaces do the same functionality.
3. **Change puzzlers** (affect [[Clean Code#4 metrics of code cleanliness|coupling and cohesion]]):
	- [Parallel Inheritance Hierarchies](https://refactoring.guru/ru/smells/parallel-inheritance-hierarchies) – every subclass creation for class A requires to create subclass for class B.
	- Divergent Change – small updates inside a class lead to extensive changes all over the class.
	- Shotgun Surgery – small updates in a class lead to a lot of small changes in a lot of other classes.
4. **Trashers** (increase code [[Clean Code#4 metrics of code cleanliness|congestion]]):
    - useless classes/wrappers/delegates.
    - duplicated, dead or unused code.
    - garbage comments.
5. **Relation entanglers** (hits [[Clean Code#4 metrics of code cleanliness|coupling and complexity]]):
	- Class/method uses another class data or methods more frequently than its own.
	- Cyclic dependency, transitive dependency.
	- Message chain: `a => b() => c() => d()`.
6. **Ugly design** (makes worse [[Clean Code#4 metrics of code cleanliness|everything]]):
	- Data storage and business logic functionalities combined in single class.
	- Several functionalities combined in a single class (business logic and scheduling).
	- Incomplete functionality of immutable or 3rd-party library class requires additional handlers.

**When to refactor:**
- Rule of Three – when you do something selfsame the third time.
- New feature implementation – it’s easier to understand written code and make it clearer for understanding in future; it’s more obvious how you can implement a feature better and easier.
- Bugfix.
- Code review (better with implementor).
