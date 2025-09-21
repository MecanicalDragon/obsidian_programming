**Жадный алгоритм** ([Greedy algorithm](https://ru.wikipedia.org/wiki/%D0%96%D0%B0%D0%B4%D0%BD%D1%8B%D0%B9_%D0%B0%D0%BB%D0%B3%D0%BE%D1%80%D0%B8%D1%82%D0%BC)) — алгоритм, заключающийся в принятии локально оптимальных решений на каждом этапе, допуская, что конечное решение также окажется оптимальным.

---
**Хвостовая рекурсия** - рекурсия, вызываемая последним выражением в функции. Преимущество её в том, что большинство компиляторов (в том числе JVM) умеют разворачивать такую рекурсию в цикл, что исключает возможность переполнения стека.

---
**Pure function** is a function that is *determined* and has *no side effects*. Determined means that for the same input data function always returns the same result. Side effect absence means that function doesn’t affect its input parameters or objects outside its scope.

---
**Аутентификация** — процедура проверки подлинности, идентификации пользователя. ^a1

**Авторизация** — предоставление пользователю прав на выполнение определённых действий, а также процесс проверки (подтверждения) данных прав при попытке выполнения этих действий. ^a2

---
**Classic Singleton** is an antipattern because it has at least two responsibilities: its primal responsibility and the creation of the singleton itself. Furthermore, it is static, that means its implementation is tightly coupled with all its consumers and can not be removed in unit testing or anywhere else. It has a global state, is not visible in class dependencies, increases coupling, and provokes to write ugly code.

