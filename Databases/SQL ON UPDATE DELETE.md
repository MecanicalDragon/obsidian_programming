```sql
CREATE TABLE CREW (
 	id INT PRIMARY KEY NOT NULL,
 	name VARCHAR (50)
);

CREATE TABLE BIKER (
 	id INT PRIMARY KEY NOT NULL,
 	name VARCHAR (50),
 	crew_id INT FOREIGN KEY CREW (id)
);
```

|                           | ON UPDATE                                                                                                                                                                             | ON DELETE                                                                                                                                                                                    |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **RESTRICT**<br>(default) | If you try to update an `id` in the `CREW` table, the engine will reject an operation if at least one `BIKER` refers to this crew.                                                    | If you try to delete an `id` in the `CREW` table, the engine will reject the operation if at least one `BIKER` refers to this crew.                                                          |
| **NO ACTION**             | Same as `RESTRICT`                                                                                                                                                                    | Same as `RESTRICT`                                                                                                                                                                           |
| **CASCADE**               | If you update an `id` in the `CREW`  table, the engine will update `crew_id` in all `BIKER` entries referring to this `CREW`, but no triggers will be activated on the `BIKER` table. | If you delete an entry in the `CREW` table, the engine will delete all referring `BIKER` entries as well. This is not safe but can be useful to make automatic cleanups in secondary tables. |
| **SET NULL**              | If you update an `id` in the `CREW` table, the engine will set all referring `BIKER`s’ `crew_id` to NULL (`crew_id` must be nullable for this option).                                | If you delete an entry in the `CREW` table, the engine will set all referring `BIKER`s’ `crew_id` to NULL (`crew_id` must be nullable for this option).                                      |
