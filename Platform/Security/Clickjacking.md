Clickjacking — атака, при которой вредоносный сайт встраивает сайт-жертву в iframe и накладывает невидимые элементы, чтобы обманом заставить пользователя кликнуть не туда.

Для защиты на сервере используются ограничивающие или запрещающие встраивание заголовки:
- `X-Frame-Options: DENY`
- `X-Frame-Options: SAMEORIGIN`
- `Content-Security-Policy: frame-ancestors 'none';`
