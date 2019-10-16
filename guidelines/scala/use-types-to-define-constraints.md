# Use types to define constraints

As types are checked by to compiler which guarantee them to be correct. It's a good idea to encode our constraints in types so we can be sure that if it compiles, it works.

In Scala we already have some types doing that:
- `Option` represents possibly empty values and replace `null`
- `Try` represents a result of computation that may fail
- `Future` represents an async result that may fail

But we can use other interesting ones such as `NonEmptyList` (see [cats implementation](https://typelevel.org/cats/datatypes/nel.html) for example).

And we can create custom ones as needed:

```scala
import scala.util.{Try, Success, Failure}

class Length(val value: Int) extends AnyVal

object Length {
  def apply[A](in: Seq[A]): Length = new Length(in.length)
  
  def from(in: Int): Try[Length] =
    if(in >= 0) Success(new Length(in))
    else Failure(new Exception(s"Unable to build a Length with a negative Int ($in)"))
}
```

This implementation guarantees that if you take a `Length` parameter, its value will be positive, not need to check for it anymore \o/
