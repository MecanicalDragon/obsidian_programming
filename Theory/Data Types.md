**Product type** - type that contains several data at the same time:
- Tuples
- Records
- OOP Objects
**Sum type** - type that can be one of several values:
- Algebraic Data Type `{ name: string } | { age: number }` (can be either name or age)
- Enum
- Sealed classes in Kotlin or Java
**Intersection type** - `{ name: string } & { age: number }` (must contain both name and age)
**Function type** - `() => T`
**Unit type** - type that is able to possess a single value (`Unit` in Kotlin)
**Void type** - `void` in Java
