# std::random_device

A nondeterministic source of randomness — hardware entropy, when the
platform provides it. Use it once to **seed** a fast PRNG like
`std::mt19937`, not as the generator itself: performance can degrade
sharply once the OS's entropy pool is drained, and on some
implementations it silently falls back to a deterministic pseudo-random
sequence instead of true entropy. (C++11)

```cpp skip
std::random_device rd;
std::mt19937 gen(rd());        // seed a fast PRNG from rd — do this
unsigned int x = rd();         // avoid: calling rd() directly and often
```

### What you provide

Nothing — `random_device` is a concrete class, not a template. Its
`result_type` is `unsigned int`.

### Guarantees and costs

- `operator()` advances the device's state and returns one
  nondeterministic value in `[min(), max()]`.
- `entropy()` estimates the entropy of the underlying source; its
  return value and exact meaning are implementation-defined.
- If no genuine nondeterministic source is available, the implementation
  may fall back to an implementation-defined pseudo-random engine — in
  that case every `random_device` object in the program can produce the
  *same* sequence. A real case of this: old MinGW-w64 builds were fully
  deterministic (bug 338, fixed since GCC 9.2).
- `operator=` is deleted — a `random_device` cannot be assigned.

### Gotchas

- Calling it repeatedly can be slow: many implementations degrade
  sharply once the entropy pool is exhausted. Use it only to seed a
  PRNG, never as the generator in a loop.
- On some implementations it's deterministic despite the name — don't
  assume real entropy without checking your target platform.

### Example

`random_device` output is nondeterministic by design, so there's no
fixed sequence to print. This seeds an engine and checks a deterministic
fact about the result instead.

```cpp
#include <iostream>
#include <random>

int main()
{
    std::random_device rd;    // nondeterministic seed source
    std::mt19937 gen(rd());   // seed a PRNG, don't generate with rd directly
    std::uniform_int_distribution<> dist(1, 6);

    int value = dist(gen);
    std::cout << "value in [1,6]: " << std::boolalpha
              << (value >= 1 && value <= 6) << '\n';
}
```

```text
value in [1,6]: true
```

### Reference

```cpp skip
class random_device;  // (since C++11)
```

**Member types** — `result_type` (C++11): `unsigned int`.

**Member functions** — constructor (C++11); `operator=` deleted
(C++11); `operator()` (C++11, generation); `entropy()`, `min()`, `max()`
(C++11, static `min`/`max`).

### See also

- **mersenne_twister_engine** (C++11) — the PRNG (via `mt19937`) that
  this is normally used to seed
- **seed_seq** (C++11) — combines several `random_device` outputs into
  a single higher-quality seed
- **uniform_int_distribution** (C++11) — shapes an engine's output into
  a bounded integer range
- **uniform_real_distribution** (C++11) — shapes it into a bounded
  floating-point range

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/random_device*
