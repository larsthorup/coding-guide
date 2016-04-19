Produce great code
====

This document describes my current thinking on which engineering practices are essential for a successful software project, and why I think so. The focus is on the technical practices, although a similar guide for the collaborative processes would be at least as valuable.


Values
----

*Freedom*: I value transparency and feedback over gateways and prevention to ensure that developers feel they can act freely. As an example I don't want to put a fixed threshold on code coverage that breaks the build if we fall below, I'd rather make sure that the code coverage trend over time is clearly visible on a development dashboard. Some gateways are acceptable though, such as failing the build on a broken test.

*Speed*: I value the simplest thing that can possibly work to avoid wasting time on features or qualities we don't need (yet). 

*Longevity*: I want to build-in scalability from the start. Scalability is part of demonstrating viability of our product. This covers both code scalability and team scalability. An example of code scalability is making the server code stateless. An example of team scalability is making it easier for new team members to join by having good automated test coverage and an enforced coding style. 

*Agility*: I want to ensure that the code can easily change and be adapted when goals and requirements change. An example is having good automated test coverage to support easy refactoring.

*Feedback*: I want to ensure that every stakeholder gets fast feedback on any decision made, whether it is a code change or a new feature. That means build-in monitoring and test automation.

Note that some of these values will often contradict eact other. You may be able to find an even simpler way to get things working by not bulding-in scalability or monitoring. So choosing what to do will often have to find a sweet balance between these different values.  


Style and naming
----

*Linting*: Enforce a coding style by choosing a widespread linter tool for our platform and a widespread linter configuration for our linter. For JavaScript, I like eslint (eslint.org) and semistandard (https://github.com/Flet/semistandard).

*Files*: Use one or more suffixes (not prefixes) for file types (like auth.scenario.test.js). Keep unit test files in the same directory as the code file they test.
 
*Product name*: Avoid using the product name for any files, modules, classes, etc, to decouple the code from marketing preferences.


Modules and dependencies
----

*Explicit*: Make many small modules and explicit dependencies between modules to ensure code coupling transparency. I am currently undecided on whether we should use an external dependency management repository (like npm for JavaScript). I am also undecided on whether we should keep each module in its own source repository, or whether to keep all modules in a single source repository.

*Dependency age*: Monitor the age of all dependencies to help minimize the technical debt of upgrading them. For JavaScript, use "npm outdated".


Quality and testing
----

*Linting*: Enforce safety checks by using the linter and configuration chosen for enforcing coding style, see above.
  
*Speed*: Spend most of your testing effort writing fast unit tests. Unit tests should be entirely in-process with no external dependencies, like file system, browser or network activity. Keep a separate suite of integration tests for more or less end-to-end testing. For JavaScript run your front-end unit tests in Node for speed and then during CI also in browsers for compatibility testing.

*Mocking*: Avoid hard coding mocks of external dependencies, but instead use auto-mocking techniques as described in this blog post (http://zealake.com/2016/03/20/dont-let-your-mocks-lie-to-you/)

*Code coverage*: Monitor code coverage. Ideally we also want to monitor code coverage incrementally (per changeset), if we can find such a tool.
 
*Test continuously*: Motivate ourselves to keep our unit tests fast by running them continuously during coding, using the test runners watch capabilties, or a dedicated continuous testrunner like Wallaby or NCrunch.

*Review*: Frequently read and comment on other peoples code, using pull requests, or pair programming.

Schemas and type checking
----

*At rest*: Use schemas for data at rest. For SQL do schema migrations in batch. For document stores keep a version property in each document for incremental or batch migrations.

*In flight*: Version APIs only when necessary, allow the use of dynamically typed languages. Catch typing errors with unit tests.  


Build and deploy
----

*Plain scripts*: Every build and deploy (local or remote) should be a single line script command. Use widespread build tools. Keep build time low.
 
*Reuse*: Reuse build scripts across modules by encapsulating them in their own modules.
