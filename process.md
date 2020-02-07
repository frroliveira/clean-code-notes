[Previous: Testing](./testing.md)

## Code Process

In practice, good practices are only good if they are applied to our software and having a process to follow can help achieve that. The process here defined is not to be confused with software processes that look at the entire lifecycle, it rather focuses on the quality of code alone. Also, it has been empirically identified hence it should be read as a suggestion.

### 1. Learn

> Do some learning tests

The suggestion is that we should start by understanding the interactions our software will have (with systems/APIs/libraries) through the use of learning tests. These tests aim to verify that we are making the correct assumptions about our dependencies in an easy and isolated way. They can take several forms, such as calling a web service manually, following a tutorial on a new technology, creating unit tests that assert expectations of a new library, etc.

### 2. Refactor

> Refactor any existing code

Alongside the learning tests we should try to understand any existing code, if we don't understand it we should refactor it into something more obvious. Trying to change code that we don't understand is a first step towards inconsistent and bogus code.

### 3. Create

> Write new code using iterative refactoring

The recommendation is to write down any initial thoughts and then iteratively refactor the code until it reads well and follows our principles. The fundamental idea here is to recognize that we can't think of a solution to a new problem in an instant (not even rockstars) and so, our best bet is to find it iteratively. This also avoids getting stuck looking for the best solution.
