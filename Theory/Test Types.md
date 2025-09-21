**Unit testing** is a form of *open-box testing*, in which the test evaluates the inner workings of the code - its structure and logic - rather than its functionality for the end user. Unit testing involves testing the individual pieces (units) of code separately from other components and in isolation from the rest of the software. Unit tests are created and performed in complete isolation from the rest of the system. Unit tests must be **FIRST**: *fast*, *isolated* (independent of each other), *repeatable* (independent of environment and resources), *self-validating* (passed or failed only as result), *timely* (written it-time with a task).

**Component testing** is a form of *closed-box testing*, meaning that the test evaluates the behavior of the program without considering the details of the underlying code. Component testing is done on the section of code in its entirety, after the development has been completed. Component testing examines use cases, so it could be considered a form of *end-to-end* (E2E) testing. End-to-end testing and component testing replicate real-life scenarios and test the system against those scenarios from a user’s perspective.

**Integration testing** – individual software modules are combined and tested as a group. Integration testing is conducted to evaluate the compliance of a system or component with specified functional requirements. It occurs after the component testing and before system testing. Integration testing takes as its input modules that have been unit tested, groups them in larger aggregates, applies tests defined in an integration test plan to those aggregates, and delivers as its output the integrated system ready for system testing.

**System testing** is testing conducted on a complete integrated system to evaluate the system's compliance with its specified requirements. System testing looks for defects both within the system components and within the whole system.

**Functional testing** is a type of *black-box testing* that bases its test cases on the specifications of the software component under test. Functions are tested by feeding them input and examining the output, and internal program structure is rarely considered. Functional testing is conducted to evaluate the compliance of a system or component with specified functional requirements and describes what the system does. Since functional testing is a type of black-box testing, the software's functionality can be tested without knowing the internal workings of the software. The term of functional testing can be applied both to *component testing* and *system testing*.

**Some more types of testing:**
- **Regression testing** – testing of the application to ensure that newly added functionality did not break existing one or that bug fixes didn’t create new bugs.
- **Smoke testing** – minimal testing of application for obvious bugs presence. Usually performed by a programmer. If an application doesn’t pass smoke tests it doesn’t make sense to push it forward to more thorough testing.
- **Monkey testing** – technique where the user tests the application by providing random inputs and checking the behavior, or seeing whether the application or system will crash. Monkey testing is usually implemented as random, automated unit tests.
- **Basis path testing** - testing strategy, that implies to test each linearly independent path through the program; in this case, the number of test cases will be equal to the [[Code Complexity|cyclomatic complexity]] of the program ([[Design Predicates]]). ^bpt
- **A/B Testing** (split testing or bucket testing) is a method of market investigation that implies that different test groups will be treated with different versions of a product to collect statistics and determine which version is better for business.

**Performance testing types:**
- **Load Testing** – test under expected load, demonstrates that the system really works and is able to scale.
- **Stress Testing** – test until the system fails to know which components start to suffer first and what resources they lack (CPU, Memory, Disk IO, Network).
- **Soak Testing** – continuous test, that will show if we can obtain problems during long system work ([[Memory Leaks]], for example).
##### See other:
- [[Design Predicates]]
