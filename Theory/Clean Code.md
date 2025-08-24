Clean code is code that is easy to read, change, reuse, and test.
## 4 metrics of code cleanliness

**Coupling**. High coupling means that your module knows way too much about the inner workings of other modules. Modules that know too much about other modules make changes hard to coordinate and make modules brittle. If Module A knows too much about Module B, changes to the internals of Module B may break functionality in Module A. Modules should be as independent as possible from other modules, so that changes to a module don’t heavily impact other modules. Class A is coupled with class B if it has fields of B, invokes B methods, has local B variables, extends B.

**Cohesion**. It shows how dense code elements that do one job reside to each other. Low cohesion classes have a lot of methods that do not invoke each other. Low cohesion would mean that the code that makes up some functionality is spread out all over your codebase. Not only is it hard to discover what code is related to your module, it is difficult to jump between different modules and keep track of all the code in your head. Perfect cohesion entity – lambda.

**Complexity**. It is determined by the number of interacting entities, number of operations in a method, inheritance depth, depth of method hierarchy, if-else branching and number of possible states.

**Congestion**. It is usually estimated by the number of fields in a class, methods, subclasses and libraries, or quantity of code either.

## 5 levels of code cleanliness

1. Formatted according to common and local code conventions
2. Easy to read:
	1. Low *complexity* and *congestion*
	2. Self-describing names of classes, methods, and variables
	3. Absence of logical paradoxes
	4. Absence of deeply nested method hierarchy
	5. Absence of garbage comments. Comments don’t describe what code does but provide additional information.
3. Easy to segregate and change:
	1. Minimal *coupling*, maximal *cohesion*
	2. No duplication
	3. Small and compact classes, that have one certain purpose and respect SR principle
	4. Methods:
		1. Compact, self-describing
		2. Do one certain job, have one certain purpose
		3. Pure, have few arguments
		4. If the method changes object, method returns it
4. Well designed
	1. Comprehensible packages and predictable classes location
	2. Compact dependencies
	3. Clear and valid architecture
	4. Self-describing names of components, that conform to the SR principle
5. Test covered
##### See other:
- [[Code Smells]]
- [[Code Complexity]]
