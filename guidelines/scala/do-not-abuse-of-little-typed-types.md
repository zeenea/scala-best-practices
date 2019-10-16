# Do not abuse of little typed types

Types are present to define and enforce constraints to help and guide developers. Some types are so flexible that they escape this mindset, for example: `Any` or `AnyRef`, but also `Map[String, String]`, `Config` or `JsValue`...

They are good to handle very flexible parts of the software but should be transformed as soon as possible in more precise types that offer better guarantees.

- `Config` type can be avoided using [PureConfig](https://pureconfig.github.io) lib
- `JsValue` should be encoded/decoded from/to an ADT automatically
- `Map[String, String]` can be replaced by a case class when key are known and limited or should have a dedicated type in the key position (Enum or Id for example)
- `Any` and `AnyRef` should be decoded using pattern matching as soon as possible

In general, theses types should not be present in domain code and should be limited to their specific transformation layer if can't be completely avoided.
