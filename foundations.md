[Previous: Readability](./readability.md)

## Code Foundations

Our programs at their core are the composition of simple constructs provided by the programming language. Similarly to building bridges and houses, we want to make sure our foundations are of good quality.

### Classes & Functions

The simplest principle about these is that

> **Classes and functions should be small**

Small will probably mean that they are easy to understand, that they implement one thing (single concept), and consequently that they are correct.

```scala
def showPassword(user: User): String =
  if (canViewPassword(user)) getPassword(user)
  else fallbackPassword

def canViewPassword(user: User): Boolean =
  user.roles.contains("ADMIN")

def getPassword(user: User): String =
  user.password

val fallbackPassword: String =
  "*********"
```

In the example above the `showPassword` function has been made small by delegating implementation to separate functions, that do one thing. If the details of the helper functions grow `showPassword` will remain small and understandable.

### Data Representation

In objected oriented programming, data encapsulation aims to hide data such that it can be freely changed when necessary. It can be accomplished by making the fields of a class `private`, which means that any logic depending on those fields must be implemented inside the same class.

```scala
case class Price(private val cents: Int) {
  def amount(): Double = cents / 100.0
}
```

Another approach to data representation is to expose it completely, instead of hiding it. This enables logic (e.g. `amount`) to be separate from the classes holding the data.

```scala
case class Price(cents: Int)

object Price {
  def amount(price: Price): Double = price.cents / 100.0
}
```

#### Polymorphic Types

Data encapsulation is commonly used together with polymorphism, as illustrated by the `Shape` example below.

```scala
trait Shape {
  def area: Double
}

case class Square(private val side: Double) extends Shape {
  override def area: Double = side * side
}

case class Rectangle(private val width: Double, private val height: Double) extends Shape {
  override def area: Double = width * height
}
```

Alternatively, the same functionality can be implemented by exposing the data held by `Square` and `Rectangle`.

```scala
trait Shape
case class Square(side: Double) extends Shape
case class Rectangle(width: Double, height: Double) extends Shape

object Geometry {

  def area(shape: Shape): Double =
    shape match {
      case square: Square => square.side * square.side
      case rectangle: Rectangle => rectangle.width * rectangle.height
    }
}
```

Analyzing the table below, we can conclude that data encapsulation "makes it easy to add new classes without changing existing functions", while exposing data "makes it easy to add new functions without changing existing classes".


| | With Data Encapsulation | Exposing Data | Example |
|-|-------------------------|---------------|---------|
| **Add New Function**   | Every class changes  | Single function changes | Add `perimeter` function |
| **Add New Class**      | Single class changes | Every function changes  | Add `Circle` class       |
| **Change Data Fields** | Single class changes | Every function changes  | `Square` is defined by two points instead of a `side` value |

[Next: SOLID Principles](./solid.md)
