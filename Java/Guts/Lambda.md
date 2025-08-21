## Лямбды после компиляции.
В первом приближении лямбды после компиляции превращаются в приватные статические методы. Это можно увидеть с помощью команды `javap -p Classname`. Но не всё так просто….

В java 7 появилось новое апи для вызова методов – [[MethodHandle]]. В чём разница с Reflection API? MethodHandle выполняет проверки типов, доступов и т.д. всего один раз – во время создания, таким образом он оптимизирует вызов методов, неизвестных во время компиляции. Reflection же делает все эти операции при каждом вызове. Основная миссия MethodHandle – работать в паре с *invokedynamic*.

## Как вызываются методы в JVM
Для разных методов компилятор генерирует разные байткод инструкции:
- **invokestatic** – статические методы
- **invokevirtual** – public и protected non-final методы инстанса
- **invokespecial** – приватные методы, конструкторы и вызовы через super
- **invokeinterface** – вызов методов интерфейса

Байткод можно посмотреть, вызвав команду для скомпилированного файла: `javap -c -v Classname.class` При первом использовании файл с байткодом загружается с диска и регистрируется внутри JVM. Нам интересны 4 сущности:

**Constant Pool** – табличка из class-файла с именами полей, методов и аргументов. Выглядит как-то так:
```
#14=Utf8             Autumn
#16=Class            #18      
#17=NameAndType      #19:#20
```

**Список байткод инструкций**
```
29: getstatic               #39
32: aload_3
33: invokevirtual           #45
```
Все конкретные методы и аргументы обозначаются через ссылки из *Constant Pool*.

**Таблица виртуальных методов класса** (Virtual Method Table или VTable) - для каждого класса создается и заполняется во время загрузки класса. Она используется для быстрого доступа к методам класса во время выполнения.

**Таблица с интерфейсами и реализациями** (Interface Method Table или ITable) - помогает обеспечивать полиморфизм и динамическое связывание при вызове методов интерфейсов. ITable создается для каждого интерфейса, реализованного классом, и содержит указатели на конкретные реализации методов интерфейса в данном классе. Когда вызывается метод интерфейса через ссылку на интерфейс, JVM использует ITable для нахождения правильной реализации метода в объекте, на который указывает ссылка.

Каждый из invoke* методов работает по своему алгоритму: для protected метода, например, нужно походить по иерархии, а для приватного нет. В некоторых invoke* методах алгоритм связывания жёстко задан внутри JVM - все структуры строятся при загрузке класса и больше не меняются, поэтому все эти методы должны заранее знать все классы и типы аргументов.

Для появившегося же в Java 7 *invokedynamic* логика связывания прописывается в отдельном методе, который может быть любым. Разберём по шагам, как это работает. 
В байткоде на месте вызова лямбды появляется инструкция: `invokedynamic #7, 0`
В пуле констант для неё находится такая запись: `#7 = InvokeDynamic #0:#8`
Идём по ссылкам и попадаем в специальную секцию `.class` файла под названием *BootstrapMethods*. Если компилятор использует invokedynamic, то он обязан прописать такую секцию. В секции бутстрап-методов видим вызов `LambdaMetafactory.metafactory(…)` Этот фабричный метод возвращает объект *CallSite*. У него есть три реализации: одна с неизменным [[MethodHandle]] и две с переменным. Больше в байткоде нет информации, но рассмотрим сам класс *LambdaMetafactory*. Класс лежит в JDK, и в нем с помощью ASM библиотеки создаётся внутренний класс. Он реализует нужный интерфейс и называется как-то так: `class Smth$$Lambda$14/0x0000000800c02460`.

Класс динамический и существует только в памяти. Создаётся объект этого класса и привязывается к MethodHandle внутри *ConstantCallSite*. Все обращения к лямбде переадресовываются этому объекту.

**Чем это лучше анонимных классов?**
- Класс создаётся и загружается динамически. На деле это получается быстрее, чем загрузка файла с диска
- Есть кэширование
- Новый класс наравне со всеми участвует в JIT оптимизациях

**Почему нельзя вынести лямбду в отдельный метод и просто вызывать его?**
>Чтобы встроить решение в остальной java код. Если в коде пишется `Predicate p = i -> x<3;` , то ожидается, что p – это объект. Его можно куда-то передать и вызвать его методы. Логика вроде «назовём это объектом, но на самом деле это метод» – это костыль. Новые фичи должны вписываться в систему, а не обходить её.

**Зачем создавать лишние классы через все эти промежуточные методы?** 
>Текущий код *LambdaMetaFactory* со временем изменится: добавится больше кэширования, семантика лямбд расширится в следующих версиях java. Если вся логика описана в JDK, гораздо легче поддерживать совместимость версий.
## Closures, method references and inline functions

Lambda expression is called *Capturing* or *Closure* if it uses variables outside its own body. It performs worse than non-capturing lambda because it needs its lexical environment for execution. Non-capturing lambda doesn’t need it, therefore it can be evaluated only once.

Conditionally all lambdas can be divided into three groups by performance: method references, non-capturing and closures ([tests](https://medium.com/@damian.kolasa/performance-implications-of-lambdas-and-method-references-when-mapping-a-stream-in-java-79f6e2da6806)). Some Oracle documentation about it: [one](https://cr.openjdk.java.net/~briangoetz/lambda/lambda-state-final.html), [two](https://cr.openjdk.java.net/~briangoetz/lambda/lambda-translation.html), [three](https://docs.oracle.com/javase/specs/jls/se13/html/jls-15.html#jls-15.27.4), [four](https://docs.oracle.com/javase/specs/jls/se13/html/jls-15.html#jls-15.13.3).

Inline function is a function that’s body can be extracted and inserted into the code in the place of function call during compilation. This reduces overhead of a function call, which becomes even more significant if the function is a closure.
