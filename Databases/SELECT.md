Чем плох `SELECT *`
- шлет больше ненужных данных по сети
- тратит ресурсы приложения на лишние данные
- может ухудшать производительность БД при больших строках в таблице

Записи таблицы хранятся на диске в страницах по 16 кб по умолчанию, и чтобы прочитать эти данные, MySql в любом случае читает с диска всю страницу и парсит её уже в памяти (в памяти она хранится в так называемом *buffer pool*) - так эффективнее. 

Однако, большие поля вроде blob или text могут храниться на другой странице (*overflow pages*). В таком случае они называются *off-page* поля. К ним же относится и varchar, если dynamic (тип строки по умолчанию) строка не помещается на страницу. Туда же переезжают и compact строки, если их размер более 768 байт (по умолчанию на один char в MySQL отводится 3 байта. Столько же дается и на указатель длины строки. 255 * 3 + 3 = 768)

Соответственно, если в таблице есть подобные поля, и размер строки достаточно большой, чтоб они выезжали в overflow pages, гранулярными запросами можно выиграть как время чтения с диска, так и место в пуле, повысив производительность БД.

Однако, намного более значительного прироста производительности можно достичь используя *покрывающий индекс*. Индексы хранятся в отдельном от таблиц пространстве, и если в индексе есть все запрашиваемые данные, MySQL вообще не будет заглядывать в таблицу, и это очень значительная экономия.
### Buffer pool

Buffer pool включает в себя:
- Новый список (New Sublist / Young List): Содержит "горячие" (часто используемые) страницы.
- Старый список (Old Sublist / Old List): Содержит "холодные" страницы.

Когда страница читается с диска, она сначала помещается в середину старого списка. Только если страница в старом списке была запрошена повторно в течение определенного периода (или при определенных условиях), она перемещается в новый список.

Часто меняющиеся данные, которые также часто запрашиваются, являются идеальными кандидатами для постоянного нахождения в Buffer Pool.
Когда происходит UPDATE или INSERT данных, соответствующие страницы (или страницы индекса) обязательно загружаются в Buffer Pool, если их там нет. Эти страницы становятся "грязными" (dirty), что означает, что их содержимое в памяти отличается от того, что на диске. InnoDB старается держать "грязные" страницы в Buffer Pool как можно дольше (в разумных пределах), чтобы накапливать изменения и сбрасывать их на диск большими партиями (групповая запись), что более эффективно. За размер пула отвечает настройка `innodb_buffer_pool_size`.
