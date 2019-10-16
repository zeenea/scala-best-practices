# Do not abuse of primitive types

This is a well known code small named [Primitive Obsession](https://www.franzoni.eu/primitive-types-are-not-your-friends/), often due to developer laziness.

When you manipulate a lot of `String`, `Int` or `Boolean`, it becomes harder and harder to know what they contain and do not make mistakes manipulating them.

A good way to know if you should create a class or just use a `String` (or `Int` or `Boolean` or any other primitive type), ask yourself, does any `String` is valid here. 
If the response is no, you should probably create a dedicated class.

A dedicated class will allow to:
- have good semantics on what things are (a `String` gives no information, but a `DatasetId` does)
- define a domain language for usual operations (and not code them again and again)
- enforce some constrains on the value (not any `String` is a correct email)

Let's take the example:

```scala
def getResources(path: String): Seq[String] = ???
```

We have no clue on what the `path` parameter may contain, it could be:
- an file uri (`file:///user/john/path/to/folder`)
- an absolute path starting with a / (`/user/john/path/to/folder`)
- an absolute path not starting with a / (`user/john/path/to/folder`)
- a relative path (`path/to/folder`)
- a folder name (`folder`)
- an url (`http://doe.com/path/to/folder`)
- a lot of other possibilities (specific protocols (ftp, ssh...), made up path (package...))
- or any invalid `String`...

And no clue what the function may return, it could be:
- a list of absolute path
- a list of relative path starting at the `path` level
- a list of names inside the path

Having a dedicated can help a lot. It can be as simple as a value type:

```scala
// `extends AnyVal` allows no runtime overhead, it will be a simple String at runtime
case class AbsolutePath(value: String) extends AnyVal

def getResources(path: AbsolutePath): Seq[AbsolutePath] = ???
```

it can add some semantic operations:

```scala
case class AbsolutePath(value: String) extends AnyVal {
  def child(name: String): AbsolutePath = AbsolutePath(value + "/" + name)
}
```

it can carry some validation to guarantee a correct value:

```scala
import scala.util.Try
// not a case class and private constructor allows to force user to use builders with validation
class AbsolutePath private(val value: String) extends AnyVal
object AbsolutePath {
  def from(in: String): Try[AbsolutePath] = ???
}
```

For numerical values, you can define usual operations and a `Numeric` instance to use common scala methods:

```scala
import scala.util.Try

case class Count(value: Long) extends AnyVal {
  def <(v: Count): Boolean = value < v.value
  def <=(v: Count): Boolean = value <= v.value
  def +(v: Count): Count = Count(value + v.value)
  def -(v: Count): Count = Count(value - v.value)
  def toInt: Int = value.toInt
}

object Count {
  def apply[A](seq: Seq[A]): Count = new Count(seq.length)

  implicit val Num: Numeric[Count] = new Numeric[Count] {
    override def plus(x: Count, y: Count): Count = Count(x.value + y.value)
    override def minus(x: Count, y: Count): Count = Count(x.value - y.value)
    override def times(x: Count, y: Count): Count = Count(x.value * y.value)
    override def negate(x: Count): Count = Count(-x.value)
    override def fromInt(x: Int): Count = Count(x)
    override def toInt(x: Count): Int = x.value.toInt
    override def toLong(x: Count): Long = x.value
    override def toFloat(x: Count): Float = x.value.toFloat
    override def toDouble(x: Count): Double = x.value.toDouble
    override def compare(x: Count, y: Count): Int = (x.value - y.value).toInt
  }
}

// this will allow:
val counts: Seq[Count] = Seq(1, 2, 3).map(Count(_))
counts.sum // Count(6)
```

With all theses techniques, you can be sure of what you manipulate, the compiler will tell you if you do a mistake and your code will be much more pleasant, thanks to dedicated methods
