**Method Reference** can be used when the result of an expression is used as an argument to the following expression.

- Reference to a static method of a class: `strings.forEach(StringUtils::capitalize);`
- Reference to an instance method of a particular object: `intStream.sorted(intComparator::compare);`
- Reference to an instance method of an arbitrary object of a particular type: `intStream.sorted(Integer::compareTo);`
- Reference to a constructor: `nameStream.map(Cat::new);`
