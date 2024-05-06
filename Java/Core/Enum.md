1. Enum constructor is always private. You cannot make it public or protected. If an enum type has no constructor declarations, then a private constructor that takes no parameters is automatically provided.
2. An enum is implicitly final, which means you cannot extend it.
3. You cannot inherit an enum from another enum or class because an enum implicitly extends **java.lang.Enum**. But an enum can implement interfaces.
4. Since enum maintains exactly one instance of each its constant, you cannot clone it. You cannot even override the clone method in an enum because **java.lang.Enum** makes it final.
5. Compiler provides an enum class with two public static methods automatically: *values()* and *valueOf(String)*. The *values* method returns an array of its constants, and *valueOf* method tries to match the String argument exactly (i.e., case sensitive) with an enum constant and returns that constant if successful, otherwise it throws **java.lang.IllegalArgumentException**. 
###### **The following are a few more important facts about java.lang.Enum which you should know:** 
- Enum implements **java.lang.Comparable** and can be added to sorted collections such as SortedSet, TreeSet, TreeMap. Enum is compared by its declaration order.
- Enum has a method *ordinal()*, which returns the index of constant i.e., the position of that constant in enum declaration.
- Enum has a method *name()*, which returns the name of this enum constant, exactly as declared in its enum declaration.
