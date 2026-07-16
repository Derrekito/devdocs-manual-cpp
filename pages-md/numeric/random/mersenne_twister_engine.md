# std::mersenne_twister_engine

You almost always want one of the two predefined aliases,
`std::mt19937` (32-bit) or `std::mt19937_64` (64-bit), not this template
directly. Seed it once — from `std::random_device` for real randomness,
or a fixed integer for reproducible output — then pass it to a
distribution to turn its raw bits into a usable range. (C++11)

```cpp skip
std::mt19937 gen;              // default seed (5489u)          (since C++11)
std::mt19937 gen(seed);        // fixed seed, reproducible       (since C++11)
std::mt19937 gen(rd());        // seeded from a random_device    (since C++11)
std::mt19937_64 gen64(seed);   // 64-bit variant                 (since C++11)
gen();                         // advance state, return next value
```

### What you provide

- Nobody writes out `mersenne_twister_engine<...>` by hand — its 13
  tuning constants (`w, n, m, r, a, u, d, s, b, t, c, l, f`) pick a
  specific Mersenne Twister variant. Use `std::mt19937` (period
  2^19937−1) or `std::mt19937_64`; the full parameter list is preserved
  in Reference for when you actually need it.
- A **seed** — a single value convertible to `result_type`, passed to
  the constructor or to `seed()`.

### Guarantees and costs

- Produces `UIntType` values uniformly over `[0, 2^w)`: high statistical
  quality, but **not cryptographically secure**.
- `operator()` advances the state and returns one value; `discard(z)`
  advances the state by `z` steps without producing output.
- Output is pinned down exactly by the standard: a default-constructed
  engine's 10,000th value is required to be `4123659995` for `mt19937`
  and `9981545732273789042` for `mt19937_64` — so, for a given seed,
  results are portable across conforming implementations.
- `operator==` compares two engines' full internal state (C++11); the
  explicit `operator!=` overload was removed in C++20 in favor of the
  compiler synthesizing it from `==`. Both engines are streamable with
  `operator<<`/`operator>>`.

### Gotchas

- Not cryptographically secure — never use it for tokens, keys, or
  anything security-sensitive, however random it looks.
- The engine's state is `n` words of `UIntType` (624 for `mt19937`);
  constructing a fresh, default-seeded engine inside a function that
  runs repeatedly resets it to the same state every time and silently
  reproduces the same sequence. Seed once, keep the engine around, and
  reuse it.
- Copying an engine copies its entire `n`-word state — fine once, but
  avoid it in a hot path.

### Example

```cpp
#include <iostream>
#include <random>

int main()
{
    std::mt19937 gen32;      // default seed
    std::mt19937_64 gen64;   // default seed

    gen32.discard(10'000 - 1);
    gen64.discard(10'000 - 1);

    std::cout << gen32() << '\n';
    std::cout << gen64() << '\n';
}
```

```text
4123659995
9981545732273789042
```

### Reference

Full declaration:

```cpp skip
template<
    class UIntType,
    std::size_t w, std::size_t n, std::size_t m, std::size_t r, UIntType a,
    std::size_t u, UIntType d, std::size_t s, UIntType b, std::size_t t, UIntType c,
    std::size_t l, UIntType f
> class mersenne_twister_engine;  // (since C++11)

using mt19937 = mersenne_twister_engine<
    std::uint_fast32_t, 32, 624, 397, 31, 0x9908b0df,
    11, 0xffffffff, 7, 0x9d2c5680, 15, 0xefc60000, 18, 1812433253>;

using mt19937_64 = mersenne_twister_engine<
    std::uint_fast64_t, 64, 312, 156, 31, 0xb5026f5aa96619e9,
    29, 0x5555555555555555, 17, 0x71d67fffeda60000,
    37, 0xfff7eee000000000, 43, 6364136223846793005>;
```

**Template parameters** — `UIntType`: the unsigned result type (`unsigned
short/int/long/long long`; anything else is UB). `w`: bit width of
generated values. `n`: degree of recurrence (state size). `m`: middle
word offset, `1 ≤ m < n`. `r`: lower bit-mask width (the twist value).
`a`: conditional xor-mask (twist matrix coefficients). `u, d, s, b, t,
c, l`: the seven components of the tempering (bit-scrambling) matrix.
`f`: the initialization multiplier. The standard requires
`0 < m ≤ n`, `2 < w`, and `r, u, s, t, l ≤ w ≤ numeric_limits<UIntType>::digits`,
with `a, b, c, d, f ≤ 2^w − 1`.

**Member types** — `result_type` (C++11): `UIntType`.

**Member functions** — constructor, `seed()` (C++11, sets the state);
`operator()` (C++11, advances and returns); `discard()` (C++11, advances
without output); `min()`/`max()` (C++11, static, output range bounds).

**Member constants** (all C++11, static) — `word_size` = `w`,
`state_size` = `n`, `shift_size` = `m`, `mask_bits` = `r`, `xor_mask` =
`a`, `tempering_u/d/s/b/t/c/l` = `u/d/s/b/t/c/l`,
`initialization_multiplier` = `f`, `default_seed` = `5489u`.

**Non-member functions** — `operator==`/`operator!=` (C++11; `!=`
removed C++20); `operator<<`/`operator>>` (C++11, stream I/O).

### See also

- **random_device** (C++11) — a nondeterministic source to seed this
  engine with
- **seed_seq** (C++11) — combines several seed values into one
- **uniform_int_distribution** (C++11) — shapes the engine's output
  into a bounded integer range
- **uniform_real_distribution** (C++11) — shapes the output into a
  bounded floating-point range
- **linear_congruential_engine** (C++11) — a simpler, cheaper, lower-
  quality alternative engine

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/mersenne_twister_engine*
