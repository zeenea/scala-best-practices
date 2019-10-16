# Scala coding guidelines

Theses guidelines are a consensus among the dev team and should be followed as much as possible.

They can be modified at any time by anyone but the modification PR should be approved by the whole dev team.

There is two kind of rules:
- the [MUST] rules should not be transgressed. If some exceptions are needed, they should be specified in the rule (and accepted by the whole team)
- the [PREFER] rules are a strong recommendation but can be transgressed if needed. A comment should explain why we need to ignore the rule.

Rules in **bold** are the most important ones.

## Table of contents

1. Language
    - [**[MUST] Never throw an exception**](guidelines/scala/never-throw-an-exception.md)
    - [**[MUST] Never use `null`**](guidelines/scala/never-use-null.md)
    - [[MUST] Never use `return`](guidelines/scala/never-use-return.md)
    - [[MUST] Never return `Unit`](guidelines/scala/never-return-unit.md)
    - [**[PREFER] Do not abuse of primitive types**](guidelines/scala/do-not-abuse-of-primitive-types.md)
    - [**[PREFER] Do not abuse of little typed types**](guidelines/scala/do-not-abuse-of-little-typed-types.md)
    - [[PREFER] Use types to define constraints](guidelines/scala/use-types-to-define-constraints.md)
    - [[MUST] Use `final` for `case class`es](guidelines/scala/use-final-for-case-classes.md)
    - [PREFER] Use value classes when possible
    - [PREFER] Do not use `lazy`
    - [MUST] Do not pattern match on `Throwable`, use `NonFatal(e)`
1. Functional programming
    - **[PREFER] Use pure functions as much as possible**
    - [PREFER] By default, use immutable values
1. Architecture
    - [**[PREFER] Split technical and business code**](guidelines/scala/split-technical-and-business-code.md)
    - [PREFER] Write a wrapper for lib that do not follow theses guidelines to adapt them
    - [MUST] Avoid using mutable shared state
    - [PREFER] Do not rely on temporal coupling
    - [MUST] Do not build an invalid object
1. Syntax
    - [MUST] Always write type for public methods and attributes
    - [PREFER] Write type for private methods and attributes
    - [PREFER] Functions should be less than 25 lines
    - [MUST] `object` attributes should start with a lower case letter to avoid conflicts with types

## Contribute

Open a PR with your rule and let's discuss.

To adopt a rule, the PR should be approved by every team member.

## References and inspirations

They are here just as a source of inspiration, they are not included in our guidelines

- [Alexandru Nedelcu best practices](https://github.com/alexandru/scala-best-practices)
- [Nicolas Rinaudo best practices](https://nrinaudo.github.io/scala-best-practices/)
- Li Haoyi's Strategic Scala Style: [Principle of Least Power](http://www.lihaoyi.com/post/StrategicScalaStylePrincipleofLeastPower.html) [Conciseness & Names](http://www.lihaoyi.com/post/StrategicScalaStyleConcisenessNames.html) [Practical Type Safety](http://www.lihaoyi.com/post/StrategicScalaStylePracticalTypeSafety.html) [Designing Datatypes](http://www.lihaoyi.com/post/StrategicScalaStyleDesigningDatatypes.html)
- Jakub Kozłowski talk: [7* sins of a Scala beginner](https://kubukoz.github.io/talks/seven-sins-of-a-scala-developer/slides) ([vidéo](https://www.youtube.com/watch?v=Z2YzCzfUNNk))
- Nicolas Rinaudo talk: [Scala Best Practices I Wish Someone'd Told Me About](https://nrinaudo.github.io/talk-scala-best-practices) ([vidéo](https://www.youtube.com/watch?v=lvlnH-uEjZA))
- [Knoldus best practices](https://blog.knoldus.com/scala-best-practices)
- [Databricks best practices](https://github.com/databricks/scala-style-guide)
- [PayPal best practices](https://github.com/paypal/scala-style-guide)
- [Twitter best practices](http://twitter.github.io/effectivescala)
