### Cyclomatic complexity
**Cyclomatic complexity** is a metric (introduced in 1976) used to indicate the complexity of a program. It is a quantitative measure of the number of linearly independent paths through a program's source code. Cyclomatic complexity is computed using the control-flow graph of the program:
- the nodes of the graph correspond to indivisible groups of commands.
- a directed edge connects two nodes if the second node command might be or might not be executed after the first node command.

In practice, cyclomatic complexity shows the number of the unique paths throw the code and can be calculated simply starting with counter = 1 and increasing this counter for every branching point (`if`, `while`, `for`, `catch`, `case`).

The main purpose of the cyclomatic complexity is [[Test Types#^unit|testability]]. CC literally says: "to achieve 100% test code coverage you need exactly X test cases". Approximate normal value: 10-15.

### Cognitive Complexity
**Cognitive Complexity** was introduced by [SonarSource](https://www.sonarsource.com/) in 2017. It computes how many mental efforts of a human are required to keep the code in mind.

Cognitive Complexity is based on 3 rules:
1. **Branching structures that ease understanding do not increase complexity**. For example:
	- 5-elements `switch/case` is easier to understand than 5 related `if-else` branches, although for the cyclomatic complexity they are the same.
	- combined conditions (`if (a && b) { doWork(); }`) are easier to understand than full-fledged branching (`if (a) { if (b) { doWork(); } }`), although for the cyclomatic complexity they are the same.
2. **Nesting increases complexity**. Every nesting level (`if` inside `for`, or `if` inside another `if`, etc.) significantly increases efforts to understand the code. So, every nesting level adds +1 to every branching point.
3. **Code flow interruptions impose penalty.**  Code jump instructions (`try-catch`, `goto`, `break`, `continue`) increment complexity.

The purpose of the cognitive complexity is improving code understandability. Approximate normal value: 10-15.
