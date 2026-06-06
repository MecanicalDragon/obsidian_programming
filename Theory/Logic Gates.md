The logical function for 2 input logical variables `(A, B)` can lead to 16 possible outputs (`2^4 = 16`). But usually there are 7 the most important (not including `BUF`):

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

This is the table of all logical functions in its canonical view - **the truth table**. In this table for 4 possible inputs of `A` and `B`: `(00, 01, 10, 11)`, the output is inverted, meaning
- the output for the input `A=0, B=0` is **the least significant bit** (the right one)
- the output for the input `A=1, B=1` is **the most significant bit** (the left one).
That is somehow justified historically, we don't care how exactly.

| #   | Name    | Formula  | OUTPUT |
| --- | ------- | -------- | ------ |
| 0   | FALSE   | 0        | 0000   |
| 1   | NOR     | ¬(A ∨ B) | 0001   |
| 2   | ¬A ∧ ¬B | ¬A ∧ ¬B  | 0010   |
| 3   | ¬A      | ¬A       | 0011   |
| 4   | ¬B ∧ ¬A | ¬B ∧ ¬A  | 0100   |
| 5   | ¬B      | ¬B       | 0101   |
| 6   | XOR     | A ⊕ B    | 0110   |
| 7   | NAND    | ¬(A ∧ B) | 0111   |
| 8   | AND     | A ∧ B    | 1000   |
| 9   | XNOR    | ¬(A ⊕ B) | 1001   |
| 10  | B       | B        | 1010   |
| 11  | A ∨ ¬B  | A ∨ ¬B   | 1011   |
| 12  | A       | A        | 1100   |
| 13  | ¬A ∨ B  | ¬A ∨ B   | 1101   |
| 14  | OR      | A ∨ B    | 1110   |
| 15  | TRUE    | 1        | 1111   |
