# Never return `Unit`

When a function returns nothing, returns a `Done` object instead of `Unit`.

The `Done` object does not exist in the Scala standard library but you can create it easily:

```scala
sealed abstract class Done

case object Done extends Done
```

Why doing that?

When returning `Unit`, the Scala compiler just ignore the returned value. For example, this code returns the last statement which is an `Int`:

```scala
def parse(s: String): Int = s.toInt
```

but if the return type is set to `Unit`, the last statement (`Int`) is ignored, you don't need to provide a `Unit` to return `Unit`:

```scala
def parse(s: String): Unit = s.toInt
```

This behavior can be problematic when this is not intended, a classical example is:

```scala
import scala.util.Try
def parse(s1: String, s2: String): Try[Unit] =
  Try(s1.toInt).map(_ => Try(s2.toInt))
```

We may expect to get a `Failure` if one of the `.toInt` fails but due to the `.map` (instead of `.flatMap`) the real return type is `Try[Try[Int]]` and the second `Try` is just hidden.

Returning `Done` will help as the code will not compile with a `.map` instead of the `.flatMap`:

```scala
import scala.util.Try
sealed abstract class Done
case object Done extends Done

def parse(s1: String, s2: String): Try[Done] =
  Try(s1.toInt).flatMap(_ => Try { s2.toInt; Done })
```

This is because `Try[Try[Unit]]` is a valid `Try[Unit]` but a `Try[Try[Done]]` is NOT a valid `Try[Done]`

To have this compiler help, you should return `Done` at the end of your computation, inside the second `Try` and not do a `.map(_ => Done)` at the end to comply with the requested return type.

Here is a BAD example of using `Done`:

```scala
import scala.util.Try
sealed abstract class Done
case object Done extends Done

def parse(s1: String, s2: String): Try[Done] =
  Try(s1.toInt).map(_ => Try(s2.toInt)).map(res => Done)
```

As you can see here, `res` can be `Int` or `Try[Int]` (depending on `.flatMap` or `.map`), it does not matter and is not caught by the compiler :(
