'Decompose by' patterns are good for the *greenfield applications*; this is a pattern for *brownfield applications*, which are big an monolithic (legacy codebase). It prescribes to create two separate applications that live side by side in the same URI space. Over time, the newly refactored application “strangles” or replaces the original application until finally you can shut off the monolithic application. The Strangler Application steps are:
- **Transform** — Create a parallel new site with modern approaches.
- **Coexist** — Leave the existing site where it is for a time. Redirect from the existing site to the new one so the functionality is implemented incrementally.
- **Eliminate** — Remove the old functionality from the existing site.
