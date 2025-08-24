Design Predicates is a method to quantify the complexity of the integration between the two units of software. Each of four types of design predicates has associated *integration complexity rating*. For the units of code that apply more than one design predicate, integration complexity ratings can be combined. A number of tests required to cover all test cases equals total complexity + 1. Design predicates are useful in [[Test Types#^bpt|basis path testing]].

- **Unconditional Call** - unit A always calls unit B. This has an integration complexity of 0
- **Conditional Call** - unit A may or may not call unit B. This integration has a complexity of 1, and needs two tests: one that calls B, and one that doesn't.
- **Mutually Exclusive Conditional Call** - unit A calls exactly one of several possible units. Integration complexity is n - 1, where n is the number of possible units to call.
- **Iterative Call** - unit A calls unit B at least once, but maybe more times. This integration has a complexity of 1 and requires two tests: one that calls unit B once, and one test that calls it more than once.
- **Combining Calls** - any integration can combine several types of calls. To calculate total complexity, it’s required to summarize all distinct call complexities.
