[About Java options and arguments](https://docs.oracle.com/en/java/javase/11/tools/java.html)

[JDK_JAVA_OPTIONS](https://docs.oracle.com/en/java/javase/11/tools/java.html#GUID-3B1CE181-CD30-4178-9602-230B800D4FAE__USINGTHEJDK_JAVA_OPTIONSLAUNCHERENV-F3C0E3BA) is only picked up by the java launcher, so use it for options that you only want to apply (or only make sense for) the java startup command. This variable appeared in JDK 9 and will be ignored by earlier JDK versions.

[JAVA_TOOL_OPTIONS](https://docs.oracle.com/en/java/javase/11/troubleshoot/environment-variables-and-system-properties.html#GUID-BE6E7B7F-A4BE-45C0-9078-AA8A66754B97) is picked up also by other java tools like `jar` and `javac` so it should be used for flags that you want to apply (and are valid) to all those jdk tools.

***JAVA_OPTIONS*** is not defined in specification so discouraged to use (may lead to implicit handling)

**3 groups of JVM options:**
- **Standard** - start with dash and supported by all JVMs (`-classpath`, `-server`, `-version`).
- **Non-standard** - start with `-Х` and define basic properties of JVM. May not work in all JVMs, but if they’re supported, they scarcely will be removed: `-Xmx`, `-Xms`.
- **Advanced** - start with `-ХХ` and leverage inner JVM mechanisms. Not supported by all JVMs, frequently being changed or removed. Some of them require additional flags. To enable experimental features flag `-XX:+UnlockExperimentalVMOptions` is obligatory.

[TieredCompilation](https://stackoverflow.com/questions/38721235/what-exactly-does-xx-tieredcompilation-do)
