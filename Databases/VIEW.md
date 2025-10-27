**VIEW** — это сохранённый SQL-запрос, который ведёт себя как таблица.
Он не хранит данные, а каждый раз при обращении выполняет исходный `SELECT`.
```
CREATE VIEW active_users AS
SELECT id, name, last_login
FROM users
WHERE active = 1;

SELECT * FROM active_users WHERE last_login > SOME_DATE();
```
Изменение данных через VIEW возможно только если представление “обновляемое” (*updatable*) — то есть, построено над одной таблицей без агрегаций (`DISTINCT`, `GROUP BY`, `JOIN` и т.п.).

**MATERIALIZED VIEW** — это физически сохранённый результат запроса — то есть, данные действительно хранятся в виде таблицы, и обновляются только при явном обновлении (*refresh*).
```
CREATE MATERIALIZED VIEW sales_summary AS
SELECT region_id, SUM(amount) AS total
FROM sales
GROUP BY region_id;

REFRESH MATERIALIZED VIEW sales_summary;
```
Главная идея в том, что ты платишь за устаревшие данные, но получаешь мгновенный доступ к результатам тяжёлого запроса.
