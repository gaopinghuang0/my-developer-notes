
## Enumerations, Structures, and Classes
1. In practice, most of the custom data types you define will be structures and enumerations, rather than classes. From [Swift LanguageGuide](https://docs.swift.org/swift-book/LanguageGuide/ClassesAndStructures.html).
2. Structures and Enumerations are *value types* while Classes are *reference types*.
3. In fact, all of the basic types in Swift—integers, floating-point numbers, Booleans, strings, **arrays and dictionaries**—are *value types*, and are implemented as structures behind the scenes.
   1. Collections defined by the standard library like arrays, dictionaries, and strings use an optimization to reduce the performance cost of copying. Instead of making a copy immediately, these collections share the memory where the elements are stored between the original instance and any copies. If one of the copies of the collection is modified, the elements are copied just before the modification.