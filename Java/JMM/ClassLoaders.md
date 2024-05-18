JVM uses 3 types of classloaders:<br>
**BootstrapClassloader** works on JVM level and loads java.lang and other basic classes.
**PlatformClassloader** (**ExtensionClassloader** before Java 9) loads not mandatory but popular modules like JDBC or JMS.
**ApplicationClassloader** (aka **SystemClassloader**) loads application classes. Program execution starts with it, loading Main class.<br>
Class searching algorithm:
- ApplicationClassloader cache
- PlatformClassloader cache
- BootstrapClassloader cache
- BootstrapClassloader (jre/lib/\*.jar)
- PlatformClassloader (jre/lib/ext/\*.jar)
- ApplicationClassloader(%classpath%)
- throw **ClassNotFoundException**

Using an algorithm like that you cannot load your own, for example, String class; even if you create a class with a package of another class you cannot access package-private fields or methods of it, because they’ll be loaded by different classloaders.<br>
