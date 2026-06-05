**Closure**, also *lexical closure* or *function closure*, is a technique for implementing *lexically scoped* *name binding* for functions in a language with *first-class functions*. 

**Name binding** is association of program data with an identifier. An identifier bound to an entity is said to reference that entity. 

A programming language is said to have **first-class functions** if it treats functions as *first-class citizens*. In a given programming language design, a **first-class citizen** is an entity which supports all the operations generally available to other entities:
- being passed as an argument
- being returned from a function
- being assigned to a variable
In languages with first-class functions, the names of functions do not have any special status; they are treated like ordinary variables of a function type.

There are two types of the *scope*:
- **Lexical scope** (also called **static scope**) means that the scope of a variable's name is the scope of the program text around the function definition.
- By contrast, in **dynamic scope**, the scope of a variable's name is the function's runtime.

This means if function `f` invokes function `g` that is declared somewhere else in the program:
- under the *lexical scope*, function `g` **does not** have access to `f`'s local variables (assuming the text of `g` is not inside the text of `f`)
- while under the *dynamic scope*, function `g` **does** have access to `f`'s local variables (since `g` is invoked from `f`).

**Example**:
```
var x = 10;
function bar() { print(x); }
function foo() { var x = 20; bar(); }
foo();
```
- In a programming language with the *lexical scope* the output is `10`, because `bar()` looks for `x` where `bar()` is declared.
- In a programming language with the *dynamic scope* the output is `20`, because `bar()` looks for `x` where `bar()` is called.

Operationally, a **closure** is a record storing a function together with its environment. The environment is a mapping associating every variable in the function's surroundings with the value or reference to which that variable *name was bound* when the closure was created. Unlike a plain function, a closure allows the function to access those captured variables through the closure's copies of their values or references, even when the function is invoked outside their scope.

Java's [[Lambda]] is a closure.
