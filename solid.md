[Previous: Code Foundations](./foundations.md)

## SOLID Principles

We want systems to grow incrementally by implementing proper separation of concerns and maximizing decoupling at each level of abstraction. The following principles serve as a tool to achieve this goal.

### Single Responsibility Principle

> A class should have only one reason to change

```scala
class FeedLineFormatter {
  def toCsv(listing: Listing): String
}
```

The `FeedLineFormatter` class above has two responsibilities (reasons to change):
1. Extracting the feed data from a Listing
2. And formatting that data as a CSV line

Mixing responsibilities not only leads to complex implementations but also makes testing harder, since the number of possibilities is greater. A possible solution is to introduce a `FeedLineFactory` class that handles the first responsibility, and re-write the formatter to accept a `FeedLine` instead.

```scala
class FeedLineFactory {
  def fromListing(listing: Listing): FeedLine
}

class FeedLineFormatter {
  def toCsv(feedLine: FeedLine): String
}
```

### Open-Closed Principle

> A software artifact should be open for extension and closed for modification

This means that we should understand what is most likely to change in our system and allow that to be extended. In the `FeedFormatter` class below, the format of the lines is _open for extension_ however the line separator is _closed for modification_.

```scala
trait FeedLineFormatter {
  def format(feedLine: FeedLine): String
}

class FeedFormatter(feedLineFormatter: FeedLineFormatter) {
  private val lineSeparator = '\n'
  def format(feed: Feed): String
}
```

### Liskov Substitution Principle

> Subtypes should not break contracts of base types

The principle is mostly broken when we try to fit a type into an existing one. For example, the `FeedLineGroup` class would break the contract of the `getLineValues` method in `FeedLine` that implies a single line.

```scala
class FeedLine {
  def getLineValues: List[String]
}

class FeedLineGroup extends FeedLine {
  def lines: List[FeedLine]
  def getLineValues: List[String] = lines.flatMap(_.getValues)
}
```

### Interface Segregation Principle

> A class should not be forced to depend on methods that doesn't use

The principle make us think about the granularity of our interfaces, the fatter they are the more likely we will have to throw UnsupportedOperationException. For example, a `ReadOnlyRepository` would not be able to implement the `save` method of the `Repository` interface.

```scala
trait Repository[V] {
  def findById(id: UUID): Option[V]
  def save(value: V): Unit
}

trait ReadOnlyRepository[V] extends Repository[V] {
  def findById(id: UUID): Option[V]
  def save(value: V): Unit = ???
}
```

### Dependency Inversion Principle

> Implementation should depend on abstractions, not on details

```scala
class UserService {
  val repository = new CassandraRepository[User]()
  def findById(id: UUID): Option[User]
}
```

The `UserService` class above imposes the use of a Cassandra database, prohibiting reuse with other repositories. Similarly to the single responsibility principle, testing the service would be harder since any bugs in the `CassandraRepository` would be propagated.

A possible solution is to define a `Repository` abstraction that the `UserService` can use, and that the `CassandraRepository` can implement.

```scala
class UserService(repository: Repository[User]) {
  def findById(id: UUID): Option[User]
}
```

[Next: Testing](./testing.md)
