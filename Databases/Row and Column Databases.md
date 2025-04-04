### Row databases

Построчные БД физически хранят строки таблицы в виде одной записи, в которой поля идут последовательно одно за другим, а за последним полем записи в общем случае идет первое поле следующей записи. Такое хранение удобно для частых операций добавления новых строк в базу данных, ведь в этом случае новая запись может быть добавлена целиком всего за один проход головки накопителя.

Однако если выбирать, например, только 3 поля из таблицы, в которой их всего 50, строки будут прочитаны целиком со всеми полями. Все эти поля будут переданы процессору, который уже отберет только необходимые для запроса, что негативно сказывается на производительности.

### Column databases

Поколоночные БД представляют данные как совокупность колонок, каждая из которых по сути является таблицей из одного поля. При этом физически на диске значения одного поля хранятся последовательно друг за другом. Такая организация данных приводит к тому, что при выполнении select в котором фигурируют только 3 поля из 50 полей таблицы, с диска физически будут прочитаны только 3 колонки, что существенно уменьшит нагрузку на чтение.

Кроме того, при поколоночном хранении данных появляется возможность сильно компрессировать данные, так как в одной колонке таблицы данные как правило однотипные и зачастую даже повторяющиеся. Однако колоночные СУБД медленно работают на запись и не подходят для транзакционных систем.
