`x & mask` эквивалентно `x % m` в том случае, если `m = 2^k`, а `mask = m - 1`, то есть `m` должна быть степенью двойки.

Если `mask = 2^k - 1`, то `x & mask` — это способ быстро (на уровне битов) получить `x%2^k`. Это свойство используется, например, в [[SecureRandom|Java Random]]:
`private static final long mask = (1L << 48) - 1;` // m = 2^48
`nextseed = (oldseed * multiplier + addend) & mask;`

Также с помощью этого свойства вычисляется бакет для entry в [[Map|HashMap]], где capacity всегда степень двойки.

---





