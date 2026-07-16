# std::uniform_real_distribution

Produces floating-point values uniformly distributed on the **half-open**
interval `[a, b)` — `b` itself is (in principle) never produced. Pair it
with an engine such as `std::mt19937` the same way as
`uniform_int_distribution`: construct once, call `d(gen)` to draw.
(C++11)

```cpp skip
std::uniform_real_distribution<> d(a, b);   // RealType defaults to double  (since C++11)
double x = d(gen);                          // draw one value in [a, b)
```

### What you provide

- **RealType** — the result type; defaults to `double`. Undefined
  behavior if it isn't `float`, `double`, or `long double`.
- **a, b** — the interval bounds; the result lands in `[a, b)`.
- **gen** — a uniform random bit generator, e.g. `std::mt19937`, passed
  to `operator()`.

### Guarantees and costs

- Density function `1 / (b - a)` over `[a, b)` — uniform floating-point
  distribution.
- Satisfies the RandomNumberDistribution requirements: `reset()`,
  `param()`, `min()`, `max()` work as in `uniform_int_distribution`.
- `operator==` compares two distributions' types and parameters (C++11);
  the explicit `operator!=` overload was removed in C++20. Streamable
  with `operator<<`/`operator>>`.

### Gotchas

- `b` can, in practice, occasionally be produced: most existing
  implementations have a known rounding bug that returns `b` itself
  (GCC #63176, LLVM #18767, MSVC STL #1074) — don't rely on the interval
  being strictly half-open.
- There's no reliable way to build a **closed** `[a, b]` distribution
  from this type: passing `std::nextafter(b, ...max())` as the upper
  bound doesn't always work, due to rounding error.
- Construct the distribution once and reuse it — don't rebuild it
  inside a loop for every draw.

### Example

Print 5 random numbers in `[1.0, 2.0)`.

```cpp
#include <iostream>
#include <random>

int main()
{
    std::mt19937 gen(42);   // fixed seed so this output is reproducible
    std::uniform_real_distribution<> dis(1.0, 2.0);

    for (int n = 0; n < 5; ++n)
        std::cout << dis(gen) << ' ';
    std::cout << '\n';
}
```

```text
1.79654 1.18343 1.77969 1.59685 1.44583
```

### Reference

```cpp skip
template< class RealType = double >
class uniform_real_distribution;  // (since C++11)
```

**Member types** — `result_type` (C++11): `RealType`. `param_type`
(C++11): the parameter-set type, per RandomNumberDistribution.

**Member functions** — constructor, `reset()` (C++11); generation via
`operator()` (C++11, optionally with an explicit `param_type` argument);
characteristics `a()`, `b()`, `param()`, `min()`, `max()` (all C++11).

**Non-member functions** — `operator==` (C++11; `operator!=` removed
C++20); `operator<<`/`operator>>` (C++11, stream I/O).

### See also

- **uniform_int_distribution** (C++11) — the integer counterpart,
  closed `[a, b]`
- **mersenne_twister_engine** (C++11) — the engine typically paired
  with this distribution, via its `mt19937`/`mt19937_64` aliases
- **random_device** (C++11) — a nondeterministic source to seed the
  engine with
- **normal_distribution** (C++11) — Gaussian-shaped output, when
  uniform isn't what you want

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/uniform_real_distribution*
