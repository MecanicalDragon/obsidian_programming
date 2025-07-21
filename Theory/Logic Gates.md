In total there are 16 possible logical functions of two variables `(A, B)`, because the inputs have 4 possible combinations `(00, 01, 10, 11)`, and each combination can give a result of 0 or 1, i.e. `2^4 = 16`. But usually there are 7 the most important:

|      |            |                                                     |
| ---- | ---------- | --------------------------------------------------- |
| AND  | `A ∧ B`    | logical AND, `A && B`                               |
| NAND | `¬(A ∧ B)` | negation AND, `!A && !B`                            |
| OR   | `A ∨ B`    | logical OR, `A \|\| B`                              |
| NOR  | `¬(A ∨ B)` | negation OR, `!A \|\| !B`                           |
| BUF  | `A`        | buffer. In schemas it just empowers the weak signal |
| NOT  | `¬A`       | inversion, `!A`                                     |
| XOR  | `A ⊕ B`    | excluding OR, true if `A != B`                      |
| XNOR | `¬(A ⊕ B)` | equivalency, true if `A == B`                       |
![](logic_gates.png)

This is the table of all logical functions in its canonical view as they are listed by their value in the **truth table**

| #   | Name    | Formula  | OUT (for IN 00, 01, 10, 11) |
| --- | ------- | -------- | --------------------------- |
| 0   | FALSE   | 0        | 0000                        |
| 1   | NOR     | ¬(A ∨ B) | 0001                        |
| 2   | ¬A ∧ ¬B | ¬A ∧ ¬B  | 0010                        |
| 3   | ¬A      | ¬A       | 0011                        |
| 4   | ¬B ∧ ¬A | ¬B ∧ ¬A  | 0100                        |
| 5   | ¬B      | ¬B       | 0101                        |
| 6   | XOR     | A ⊕ B    | 0110                        |
| 7   | NAND    | ¬(A ∧ B) | 0111                        |
| 8   | AND     | A ∧ B    | 1000                        |
| 9   | XNOR    | ¬(A ⊕ B) | 1001                        |
| 10  | B       | B        | 1010                        |
| 11  | A ∨ ¬B  | A ∨ ¬B   | 1011                        |
| 12  | A       | A        | 1100                        |
| 13  | ¬A ∨ B  | ¬A ∨ B   | 1101                        |
| 14  | OR      | A ∨ B    | 1110                        |
| 15  | TRUE    | 1        | 1111                        |
In this table output is inverted as it is often applied in Boolean algebra and digital logic when enumerating functions — *the bit order is reversed*: the **least significant bit (B)** comes first, and the **most significant bit (A)** comes second. So, the **OUT** here is for `f(B,A)`, not for `f(A,B)`