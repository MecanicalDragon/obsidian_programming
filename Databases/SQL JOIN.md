![](sql_join.png)

```sql
CREATE TABLE BIKES (name VARCHAR (50), id INT);  
CREATE TABLE RIDERS (name VARCHAR (50), id INT);  
  
INSERT INTO BIKES VALUES('Yamaha', 1);  
INSERT INTO BIKES VALUES('Honda', 1);  
INSERT INTO BIKES VALUES('Suzuki', 2);  
INSERT INTO BIKES VALUES('Kawasaki', null);  
  
INSERT INTO RIDERS VALUES('Stan', 1);  
INSERT INTO RIDERS VALUES('Yuri', 1);  
INSERT INTO RIDERS VALUES('Georgiy', 3);  
INSERT INTO RIDERS VALUES('Viktor', null);  
  
SELECT b.name, r.name FROM bikes b LEFT JOIN riders r ON b.id = r.id; -- L1
SELECT b.name, r.name FROM bikes b LEFT JOIN riders r ON b.id = r.id WHERE r.id IS NULL; -- L2

SELECT b.name, r.name FROM bikes b RIGHT JOIN riders r ON b.id = r.id; -- R1
SELECT b.name, r.name FROM bikes b RIGHT JOIN riders r ON b.id = r.id WHERE b.id IS NULL; -- R2

SELECT b.name, r.name FROM bikes b FULL OUTER JOIN riders r ON b.id = r.id; -- FO1
SELECT b.name, r.name FROM bikes b FULL OUTER JOIN riders r ON b.id = r.id WHERE b.id IS NULL OR r.id IS NULL; -- FO2

SELECT b.name, r.name FROM bikes b INNER JOIN riders r ON b.id = r.id; -- IJ
```

| L1, L2                                                                                                                                          | R1, R2                                                                                                                                        | FO1                                                                                                                                            | FO2, IJ                                                                                                                                        |
| ----------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| Yamaha - Stan<br>Yamaha - Yuri<br>Honda - Stan<br>Honda - Yuri<br>Suzuki - null<br>Kawasaki - null<br>`***`<br>Suzuki - null<br>Kawasaki - null | Yamaha - Stan<br>Yamaha - Yuri<br>Honda - Stan<br>Honda - Yuri<br>null - Georgiy<br>null - Viktor<br>`***`<br>null - Georgiy<br>null - Viktor | Yamaha - Stan<br>Yamaha - Yuri<br>Honda - Stan<br>Honda - Yuri<br>Suzuki - null<br>Kawasaki - null<br>null - Georgiy<br>null - Viktor<br>`***` | Suzuki - null<br>Kawasaki - null<br>null - Georgiy<br>null - Viktor<br>`***`<br>Yamaha - Stan<br>Yamaha - Yuri<br>Honda - Stan<br>Honda - Yuri |
