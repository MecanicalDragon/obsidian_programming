**Aggregate Pattern** — это концепция из **Domain-Driven Design (DDD)**; её суть — группировать связанные данные и операции над ними в *единицу целостности*, чтобы избежать гонок, дублирования логики и несогласованных изменений.

Ключевая идея в том, что агрегат — это кластер объектов, который:
- изменяется как единое целое,
- имеет границы консистентности,
- управляется через один корень (*Aggregate Root*).

### Структура аггрегата

1. **Aggregate Root** — главный объект (сущность), через который осуществляется доступ и операции с агрегатом. Он гарантирует, что все инварианты внутри агрегата сохраняются.  
2. **Entities (сущности)** — объекты с идентичностью внутри агрегата, но не могут жить отдельно от него.
3. **Value Objects (значения)** — объекты без идентичности, определяются только набором атрибутов.

Допустим, у нас есть банковский счёт.

**Aggregate Root:** `Account`  
**Внутри агрегата:**
- `Transaction` (entity)
- `Money` (value object)

Все операции — депозит, снятие, перевод — выполняются через `Account`. Нельзя напрямую изменять транзакции или баланс — только через методы `Account.deposit()`, `.withdraw()`. Это гарантирует инварианты вроде:
- Баланс не может уйти в минус.
- Каждая транзакция логируется корректно.

### Правила проектирования агрегатов

1. **Малые границы.** Лучше несколько маленьких агрегатов, чем один огромный.
2. **Один Aggregate Root.** Только он отвечает за изменение состояния.
3. **Ссылки по идентификатору.** Агрегаты не должны содержать прямые ссылки друг на друга - только ID, чтобы избежать каскадных загрузок и сложных зависимостей.
4. **События для взаимодействия.** Если один агрегат должен повлиять на другой — публикуется событие (event-driven подход).

### Выгоды

1. **Consistency boundary** — агрегат определяет, какие данные должны быть атомарно согласованы. В рамках одного агрегата можно использовать транзакции ([[ACID]]), а между агрегатами — [[BASE|eventual consistency]].
2. **Concurrency control** — проще управлять параллельными изменениями (например, с помощью optimistic locking на уровне aggregate root).
3. **Scalability** — агрегаты можно изолировать и хранить в разных сервисах или базах.
4. **[[Event Sourcing]] / [[CQRS]]** — агрегаты естественно вписываются как единицы, которые производят *domain events* (например, `AccountCredited`, `MoneyWithdrawn`).

---
- [aggregate pattern](https://medium.com/@philsarin/whats-the-point-of-the-aggregate-pattern-741a3132da5c#id_token=eyJhbGciOiJSUzI1NiIsImtpZCI6ImNlYzEzZGViZjRiOTY0Nzk2ODM3MzYyMDUwODI0NjZjMTQ3OTdiZDAiLCJ0eXAiOiJKV1QifQ.eyJpc3MiOiJodHRwczovL2FjY291bnRzLmdvb2dsZS5jb20iLCJuYmYiOjE2NDg5NjYxMzEsImF1ZCI6IjIxNjI5NjAzNTgzNC1rMWs2cWUwNjBzMnRwMmEyamFtNGxqZGNtczAwc3R0Zy5hcHBzLmdvb2dsZXVzZXJjb250ZW50LmNvbSIsInN1YiI6IjEwMjA3Nzc2MjE1NzM0MjcyMDMyNCIsImVtYWlsIjoiZ2FmZnN0cmFuZ2VyQGdtYWlsLmNvbSIsImVtYWlsX3ZlcmlmaWVkIjp0cnVlLCJhenAiOiIyMTYyOTYwMzU4MzQtazFrNnFlMDYwczJ0cDJhMmphbTRsamRjbXMwMHN0dGcuYXBwcy5nb29nbGV1c2VyY29udGVudC5jb20iLCJuYW1lIjoi0KHRgtCw0L3QuNGB0LvQsNCyINCi0YDQtdGC0YzRj9C60L7QsiIsInBpY3R1cmUiOiJodHRwczovL2xoMy5nb29nbGV1c2VyY29udGVudC5jb20vYS0vQU9oMTRHajREWlUzM1lyMHpsdEF6bzRIR1ltSW5ib2E4RDRfYlBFRXRTQnY0UT1zOTYtYyIsImdpdmVuX25hbWUiOiLQodGC0LDQvdC40YHQu9Cw0LIiLCJmYW1pbHlfbmFtZSI6ItCi0YDQtdGC0YzRj9C60L7QsiIsImlhdCI6MTY0ODk2NjQzMSwiZXhwIjoxNjQ4OTcwMDMxLCJqdGkiOiJlNTE4NDE0NDBmYmJiZjk1ZWEwMjI0OTZlMmU1MzNhMzExNDdhN2Q3In0.Rz5kjYFmeakHv8eK8RsChi-sqY-pMpcPF_18uU5OP3pNgYE9YXzi6agJdTOiD6AFtsnRpbTfIUG0vDlm342n1B8aFkoiHzfB1CWX7rz_F00Duxsc8ao19yX1C65ekN7ro_C6nmUzPvMcVxzmMGNI-ZLKNP7c-IAMtis3f_sfuTRKoqz_KUbI9baMYaR7moTa3bFQaE7p3kki5rxu3nMsMeAap4Z9oyrd0zIzXw9j329GG_0eFs8hqSQ5fcXe_741ycz1t9R7toEzljMI3blfO-cIr5G7wdJ5U7G5kR7Hk1xWjJkCguy9PA9l5QFMIbwJrdCNl4q5OLSnDgRAjKm3zQ)
- [aggregate pattern](https://dzone.com/articles/aggregate-pattern)
