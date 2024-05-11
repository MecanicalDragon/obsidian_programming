**Functional Interface** is an interface with a sole abstract method.<br>
- **Function (BiFunction)** - *apply()*. Interface accepts 1 (2) argument(s) of the first type and converts it (them) to the second specified type value.
	- *compose()* - function before; *then()* - function after.
- **UnaryOperator (BinaryOperator)** - *apply()*. Interface is just a Function (BiFunction) for a single type.
	- UnaryOperator: *List.replaceAll(uo)*; BinaryOperator: *Stream.reduce(bo)*.
- **Supplier** - *get()*. Interface just supplies a new element.
- **Consumer (BiConsumer)** - *accept()*. Interface just applies a side effect to the argument(s).
- **Predicate (BiPredicate)** - *test()*. Interface evaluates a boolean expression based on accepted elements.
<br>Since primitive types can’t be generic arguments there exist a lot of predefined functional interfaces that work with it.<br>
**Method Reference** can be used when the result of an expression is used as an argument to the following expression.<br>
- Reference to a static method of a class: `strings.forEach(StringUtils::capitalize);`
- Reference to an instance method of a particular object: `intStream.sorted(intComparator::compare);`
- Reference to an instance method of an arbitrary object of a particular type: `intStream.sorted(Integer::compareTo);`
- Reference to a constructor: `nameStream.map(Cat::new);`
