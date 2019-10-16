# Use `final` for case classes

Sadly Scala case classes are not final and extending a case class can lead to surprising behavior...

## Equal inheritance

If a class extends a case class but do not override `equals` or other case class generated methods, it will inherit from it which can be surprising:

```scala
case class Parent(name: String)
class Child(name: String, age: Int) extends Parent(name)

val c1 = new Child("name", 10)
val c2 = new Child("name", 12)
val eq = c1 == c2     // => true           we can expect false here (value equality with different age or entity equality with different object)
val str = c1.toString // => "Parent(name)" which is strange for a `new Child("name", 10)`
```

## New sealed train implementation

`sealed trait`s are used to know all implementations of the `trait` at compile time and have some exhaustive pattern matching.

```scala
sealed trait MyEnum
case class Value1(value: String) extends MyEnum
case class Value2(value: Int) extends MyEnum

// in an other file...
class OtherValue(value: Double) extends Value1(value.toString)

// so you can do
val strange: MyEnum = new OtherValue(2.0)
val name = strange.getClass.getSimpleName // "OtherValue", which is strange for a MyEnum object

// thankfully, it does not break pattern matching
val res = strange match {
  case Value1(v1) => v1
  case Value2(v2) => v2.toString
}
// res is "2.0"
```
