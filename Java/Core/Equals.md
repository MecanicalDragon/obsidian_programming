**Контракт equals**:
- Рефлективность: `if (x != null) x.equals(x) == true`
- Симметричность: `if (x.equals(y) == true) then y.equals(x) == true`
- Транзитивность: `if (x.equals(y) == true && y.equals(z) == true) then x.equals(z) == true`
- Согласованность: `x.equals(y)` при повторном вызове без изменений даст тот же результат.
- Сравнение на null: `x.equals(null) == false` всегда.