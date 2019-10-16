# Never use `null`

You must not use `null`, prefer `Option[A]` instead.

`null` is known as [The-Billion-Dollar-Mistake](https://www.infoq.com/presentations/Null-References-The-Billion-Dollar-Mistake-Tony-Hoare) because it is very error prone.
The compiler can't track what can be null and verify that nullable objects are checked before being used.

The only solution to be sure to not make errors is a bit extreme: DO NOT ALLOW ANY `NULL` IN YOUR PROGRAM!!!

Using `Option[A]` allows to know what can be absent and helps you avoiding mistakes.

Here is an example:

```scala
def getName(path: Option[String]): Option[String] =
  path.map(_.split("/").last)
```

or an other:

```scala
def getName(path: Option[String]): String =
  path.getOrElse("").split("/").last
```

it is not recommended to use `isEmpty` and `get` as it rely on some "knowledge" that the `Option[A]` is not empty in a specific context:

```scala
def getName(path: Option[String]): String =
  if(path.isEmpty) {
    ""
  } else {
    path.get.split("/").last
  }
```

and of course, it is forbidden to pass `null` argument so no need to check for nullity:

```scala
def getName(path: String): String =
  path.split("/").last
```

this code is considered as incorrect:

```scala
def getName(path: String): String =
  if(path == null) {
    ""
  } else {
    path.split("/").last
  }
```

## Some tips

Combining multiple `Option`s can be easily done use for-comprehensions:

```scala
val nameOpt: Option[String] = Some("John")
val ageOpt: Option[Int] = Some(42)

val res: Option[String] = for {
  name <- nameOpt
  age <- ageOpt
} yield s"$name is $age years old"
// if every Option is Some, the result will be Some. Otherwise it will be None.
```

Since `Option[A]` can be automatically converted to an `Iterable` so you can see it as a collection of 0 or 1 element.

You can use `.flatMap` on a collection to eliminate `None` elements:

```scala
val list = Seq(1, 2, 3, 4, 5)
list.flatMap(i => if(i % 2 == 0) Some(i.toString) else None)
// Seq("2", "4")
```

## Other references

This rule is also present in other best practice guides:
- [Alexandru Nedelcu best practices](https://github.com/alexandru/scala-best-practices/blob/master/sections/2-language-rules.md#29-must-not-use-null)
- [Nicolas Rinaudo best practices](https://nrinaudo.github.io/scala-best-practices/unsafe/avoid_null.html)
