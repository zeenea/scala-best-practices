# Never use `return`

As Scala automatically returns the last expression of a block, you just don't need `return`.

As the `return` exit immediately the function, it increase the complexity of the function and may cause some hard-to-understand behaviors.

Moreover, it has some unwanted behaviors, for example inside an anonymous function or a lambda. For example, this code compile fine:

```scala
def asStrings(list: Seq[Int]): Seq[String] =
  list.map(i => i.toString)
```

but not this one:

```scala
def asStrings(list: Seq[Int]): Seq[String] =
  list.map(i => return i.toString)
// Error:(2, 32) type mismatch;
// found   : String
// required: Seq[String]
// list.map(i => return i.toString)
```

neither that one:

```scala
def asStrings(list: Seq[Int]): String =
  list.map(i => return i.toString)
// Error:(2, 17) type mismatch;
// found   : Seq[Nothing]
// required: String
// list.map(i => return i.toString)
```

and not even this one:

```scala
def asStrings(list: Seq[Int]): Seq[Nothing] =
  list.map(i => return i.toString)
// Error:(2, 32) type mismatch;
// found   : String
// required: Seq[Nothing]
// list.map(i => return i.toString)
```

## Other references

This rule is also present in other best practice guides:
- [Alexandru Nedelcu best practices](https://github.com/alexandru/scala-best-practices/blob/master/sections/2-language-rules.md#21-must-not-use-return)
- [Nicolas Rinaudo best practices](https://nrinaudo.github.io/scala-best-practices/referential_transparency/avoid_return.html)
