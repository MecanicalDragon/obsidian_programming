**Spring AOP** and **AspectJ** have different goals.

**Spring AOP** aims to provide a simple AOP implementation across Spring IoC. It is not intended as a complete AOP solution – it can only be applied to beans that are managed by a Spring container. Spring AOP makes use of runtime weaving and can be used only for method proxying in special join points.

**AspectJ** is the original AOP technology which aims to provide a complete AOP solution. It is more robust but also significantly more complicated than Spring AOP. AspectJ can be applied across all domain objects. AspectJ makes use of three different types of weaving:
- **Compile-time weaving**. The AspectJ compiler takes as input both the source code of the class and all its aspects, and produces woven class files as output. For this type of weaving a specific *acj compiler* is used.
- **Post-compile weaving**. Also known as *binary weaving*. It is used to weave existing class files and JAR files with aspects on a byte code level. Special AspectJ utilities (mvn and gradle plugins, command-line tools) are required for this.
- **Load-time weaving**. It is almost likewise the binary weaving, but weaving is postponed until a class loader loads class files to the JVM memory. A specific [[Java Agents|--javaagent]] is required to perform this type of weaving. This is the most popular case for AspectJ weaving.

**AspectJ** can obtain join points for constructors, method calls, static and object initializers, fields and a lot of other things. In particular, it is able to pointcut method calls within the same bean. Aspects don’t even need Spring infrastructure to work, but we can use AspectJ annotations inside a Spring container to cover its functionality with Spring AOP’s runtime weaving. In this case it will take much more time than even default Spring proxying (250ns per invocation vs 10 ns for dynamic proxy or CGLIB). That’s why if you want to intercept everything it’s better to write your custom BPP (250 ns is only 8 times smaller, than 2 bytes HDD writing). Compile-time woven aspects of AspectJ are about [8 to 35 times faster](https://web.archive.org/web/20150520175004/https:/docs.codehaus.org/display/AW/AOP+Benchmark) than runtime woven components of Spring AOP.

**AspectJ core concepts:**
- **Aspect** – functional unit that cuts across multiple objects. Each aspect focuses on a specific cross cutting functionality.
- **Join point** - a point of aspect logic inception in the code.
- **Advice** - action taken by an aspect at a particular join point.
- **Pointcut** - regular expression that matches join points. Advices are associated with a pointcut expression and run at any join point that matches the pointcut.
