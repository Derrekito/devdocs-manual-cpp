# std::uniform_int_distribution

Produces integers uniformly distributed on the **closed** interval
`[a, b]` — both endpoints included, each with equal probability.
Construct a distribution once and call it with a random engine such as
`std::mt19937`; that engine-plus-distribution pairing is the idiom to
reach for whenever you need a bounded random integer. (C++11)

```cpp skip
std::uniform_int_distribution<> d(a, b);   // IntType defaults to int  (since C++11)
int x = d(gen);                            // draw one value in [a, b]
```

### What you provide

- **IntType** — the result type; defaults to `int`. Undefined behavior
  if it isn't one of `short, int, long, long long` or their `unsigned`
  counterparts — narrower integer types like `char` don't qualify.
- **a, b** — the inclusive bounds, given to the constructor (or read
  back / changed later via `a()`, `b()`, `param()`).
- **gen** — a uniform random bit generator, e.g. `std::mt19937`, passed
  to `operator()`.

### Guarantees and costs

- Every integer `i` in `[a, b]` has probability `1 / (b - a + 1)` — the
  discrete uniform distribution.
- Satisfies the RandomNumberDistribution requirements: `reset()` clears
  any internal state cached between draws; `param()` gets or sets the
  `(a, b)` pair; `min()`/`max()` report the current bounds.
- `operator==` compares two distributions' types and parameters (C++11);
  the explicit `operator!=` overload was removed in C++20 in favor of
  the compiler synthesizing it from `==`. Streamable with
  `operator<<`/`operator>>`.

### Gotchas

- `IntType` excludes narrow character types (`char`, `signed char`,
  `unsigned char`) and `bool` — despite being integers, instantiating
  with one is undefined behavior; use `int` or wider.
- `d(gen)` is cheap to call repeatedly — construct `d` once outside a
  loop instead of rebuilding it for every draw.

### Example

This program simulates throwing a 6-sided die.

```cpp
#include <iostream>
#include <random>

int main()
{
    std::mt19937 gen(42);   // fixed seed so this output is reproducible
    std::uniform_int_distribution<> distrib(1, 6);

    for (int n = 0; n != 10; ++n)
        std::cout << distrib(gen) << ' ';
    std::cout << '\n';
}
```

```text
3 5 6 2 5 5 4 4 1 3
```

### Reference

```cpp skip
template< class IntType = int >
class uniform_int_distribution;  // (since C++11)
```

**Member types** — `result_type` (C++11): `IntType`. `param_type`
(C++11): the parameter-set type, per RandomNumberDistribution.

**Member functions** — constructor, `reset()` (C++11); generation via
`operator()` (C++11, optionally with an explicit `param_type` argument
overriding the stored bounds for that one call); characteristics `a()`,
`b()`, `param()`, `min()`, `max()` (all C++11).

**Non-member functions** — `operator==` (C++11; `operator!=` removed
C++20); `operator<<`/`operator>>` (C++11, stream I/O).

### See also

- **mersenne_twister_engine** (C++11) — the engine typically paired
  with this distribution, via its `mt19937`/`mt19937_64` aliases
- **uniform_real_distribution** (C++11) — the floating-point
  counterpart, half-open `[a, b)`
- **random_device** (C++11) — a nondeterministic source to seed the
  engine with
- **discrete_distribution** (C++11) — weighted choice among discrete
  outcomes, when uniform isn't what you want

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/uniform_int_distribution*
