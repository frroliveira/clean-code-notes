[Previous: SOLID Principles](./solid.md)

## Testing

Automated tests are very useful as they avoid manual repetition and provide documentation of expectations. They can also give confidence to make changes, however unclean tests have the opposite effect and can become a liability. This means that

> Test code should be just like production code

It should be readable and it should focus on a single concept. Tests should be fast to run (otherwise won't be run), they should be independent, repeatable in any environment and fully-automated (otherwise prone to error). Threaded code, in particular, should be tested with several configurations and tested using induced failures.

![Testing](./images/tests.png)

Just like production code, tests should be refactored. When refactoring or writing tests, aiming to keep the tests passing at all times will avoid situations where nothing works. When we make changes to the code or tests, we are validating the whole system not only the new feature so,

> Don't ignore a failing test

The practice of Test Driven Development (TDD) is recommended since it encourages writing code that is testable and writing only the strictly necessary code.

[Next: Code Process](./process.md)
