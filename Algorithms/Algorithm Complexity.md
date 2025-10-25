
- Constant time: `O(1)`
- Logarithmic time: `O(log n)`
- Polylogarithmic time: `O(log²n)`
- Sublinear time: `O(nᵏ)` for `k<1` (`O(√n) || o(n)`)
- Linear time: `O(n)`
- Linear-logarithmic time: `O(n·log n)`
- Quasilinear time: `O(n·logᵏn)`
- Polynomial time: `O(nᵏ)`
- Exponential time: `O(kⁿ)`
- Factorial time: `O(n!)`
- [Others](https://en.wikipedia.org/wiki/Time_complexity)

![[alg_complexity.png]]

*Big O* can be considered as `<=`; *Small o* can be considered as `<`:
- `O(f) == const * O(f)`; `o(f) == const * o(f)`;
- `O(f) == O(const * f)`; `o(f) == o(const * f)`;
- `O(f) == o(f) == O(O(f)) == o(o(f)) == o(O(f)) == O(o(f))`;
- `O(f) + O(f) == O(f)`; `o(f) + o(f) == o(f)`; `O(f) + o(f) == O(f)`;
- `O(f) * O(g) == O(fg)`; `o(f) * o(g) == o(fg)`; `O(f) * o(g) == o(fg)`;
