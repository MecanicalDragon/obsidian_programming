**Checked exceptions**
**Pros** (Gosling):
- You always clearly see, what problems can appear
- You always must handle it
 
**Cons** (Eckel):
- Problems with Lambdas
- A lot of throws/tries/catches
- Provoke antipattern usage

How to throw a checked exception from a method and not denote it in the method signature [described here](https://www.gamlor.info/wordpress/2010/02/throwing-checked-excpetions-like-unchecked-exceptions-in-java/)

---
## ClassNotFound & NoClassDefFound

**ClassNotFoundException** is a ***Checked Exception***; it occurs when an application tries to load a class through its fully-qualified name and can’t find its definition in the classpath. It occurs mainly during attempts to load classes using `ClassLoader.findSystemClass()`, `ClassLoader.loadClass()`, `Class.forName()`, or in some sneaky cases with different dependency versions in the classpath.

**NoClassDefFoundError** is a Fatal ***Error***; it occurs when JVM cannot find a definition of the class while trying to instantiate it with new keyword or loading a class with a method call. It occurs when the compiler has successfully compiled the class, but Java runtime cannot locate the class file. It usually happens when an exception is thrown in a static block or initializing static fields of the class, so class initialization fails.
