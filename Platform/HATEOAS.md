Термин **HATEOAS** означает фразу «Hypermedia As The Engine Of Application State» (Гипермедиа как двигатель состояния приложения). Он развивает далее идеологию [[REST]].

Как правило, когда мы выполняем запрос REST, мы получаем только данные, а не какие-либо действия с ними. Вот где HATEOAS восполняет пробел. Запрос HATEOAS позволяет вам не только отправлять данные, но и указывать связанные действия:

```
HTTP/1.1 200 OK
Content-Type: application/xml
Content-Length: ...

<?xml version="1.0"?>
<account>
  <account_number>12345</account_number>
  <balance currency="usd">100.00</balance>
  <link rel="deposit" href="https://bank.example.com/accounts/12345/deposit" />
  <link rel="withdraw" href="https://bank.example.com/accounts/12345/withdraw" />
  <link rel="transfer" href="https://bank.example.com/accounts/12345/transfer" />
  <link rel="close" href="https://bank.example.com/accounts/12345/close" />
</account>
```

Когда вы отправляете этот запрос для получения данных учетной записи, вы получаете оба:
- Номер счета и данные баланса
- Ссылки, которые обеспечивают действия, чтобы сделать депозит/снятие/перевод/закрытие

С HATEOAS запрос на REST ресурс дает как данные, так и действия, связанные с данными.

Единственная важная причина для HATEOAS — слабая связь (loose coupling). Если потребителю службы REST необходимо жестко закодировать все URL-адреса ресурсов, он тесно связан с реализацией вашей службы. Вместо этого, если вы вернете URL-адреса, которые он может использовать для действий, он будет слабосвязанным. Нет строгой зависимости от структуры URI, так как она указана и используется в ответе.
