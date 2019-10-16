# Never throw an exception

Do not throw exceptions in your code, avoid using a function that throws when you can. If not possible `catch` the exception an return a relevant result.

In Scala, exceptions are not documented and very hard to track (just like `null`), so you can't be sure everything is correctly handled and given a big enough program, you can be sure something is NOT correctly handled.

Exceptions, like every side-effect, are breaking [referential transparency](https://nrinaudo.github.io/scala-best-practices/definitions/referential_transparency.html) which makes understanding code really hard and refactoring very fragile.

Instead of throwing exceptions, use a specific type to represent the function semantics:

- `Option[A]` when something can be present or not
- `Either[E, A]` when possible errors are limited
- `Try[A]` when you don't know what may fail

Common throwing functions and how to replace them:

- `Seq[A].head` => `Seq[A].headOption`
- `Seq[A].tail` => `Seq[A].drop(1)`
- `Seq[A].last` => `Seq[A].lastOption`
- `Seq[A].reduce` => `Seq[A].reduceOption` or `Seq[A].fold`
- `Option[A].get` => `Option[A].getOrElse`
- `Try[A].get` => `Try[A].getOrElse`

## Other references

This rule is also present in other best practice guides:
- [Nicolas Rinaudo best practices](https://nrinaudo.github.io/scala-best-practices/referential_transparency/avoid_throwing_exceptions.html)
- [Alexandru Nedelcu best practices](https://github.com/alexandru/scala-best-practices/blob/master/sections/2-language-rules.md#27-must-not-throw-exceptions-for-validations-of-user-input-or-flow-control)
