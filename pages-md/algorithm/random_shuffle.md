# std::random_shuffle, std::shuffle

Use `std::shuffle` (C++11) with a real random-number engine, such as
`std::mt19937` — `std::random_shuffle` was deprecated in C++14 and
removed in C++17, because its iterator-only form usually relied on the
deprecated `std::rand` and hidden global state. Both reorder a range so
that every possible permutation is equally likely.

```cpp skip
std::shuffle(first, last, g);          // g: a UniformRandomBitGenerator (since C++11)
std::random_shuffle(first, last);      // deprecated C++14, removed C++17
std::random_shuffle(first, last, r);   // deprecated C++14, removed C++17
```

### What you provide

- **first, last** — random-access iterators (ValueSwappable and
  LegacyRandomAccessIterator).
- **g** — a UniformRandomBitGenerator (e.g. `std::mt19937`) whose result
  type converts to the iterators' `difference_type`. Construct it once
  and reuse it — this is the "real engine" the modern overload needs.
- **r** (removed overloads only) — a callable returning a value in
  `[0, n)` when invoked as `r(n)`.

### Guarantees and costs

- Linear in the distance between `first` and `last`.
- Every permutation of the range has equal probability of appearing.
- No algorithm is mandated by the standard: the same engine and seed can
  still produce different results on different standard library
  implementations.

### Gotchas

- `std::random_shuffle` no longer exists in C++17 — code that calls it
  won't compile; switch to `std::shuffle` with an engine like
  `std::mt19937`.
- Constructing (or reseeding) a new engine on every call, or per element,
  defeats the randomness and is slow — build one engine and reuse it.
- Results aren't portable across standard library implementations, even
  with an identical seed.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <random>
#include <vector>

int main()
{
    std::vector<int> v{1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    std::vector<int> original = v;

    std::mt19937 g(12345);   // fixed seed so this check is reproducible
    std::shuffle(v.begin(), v.end(), g);

    std::sort(v.begin(), v.end());
    std::cout << (v == original ? "same elements after shuffle\n"
                                 : "different elements after shuffle\n");
}
```

```text
same elements after shuffle
```

### Reference

Full declarations:

```cpp skip
template< class RandomIt >
void random_shuffle( RandomIt first, RandomIt last );  // (1) (deprecated in C++14) (removed in C++17)
template< class RandomIt, class RandomFunc >
void random_shuffle( RandomIt first, RandomIt last, RandomFunc& r );  // (until C++11)
template< class RandomIt, class RandomFunc >
void random_shuffle( RandomIt first, RandomIt last, RandomFunc&& r );  // (since C++11) (deprecated in C++14) (removed in C++17)
template< class RandomIt, class URBG >
void shuffle( RandomIt first, RandomIt last, URBG&& g );  // (3) (since C++11)
```

`std::remove_reference_t<URBG>` must meet UniformRandomBitGenerator. A
retroactive defect report (LWG 395) clarified that overload (1)'s source
of randomness is implementation-defined and may use `std::rand`; another
(LWG 552) required `r` to actually be overload (2)'s source of
randomness.

### See also

- **next_permutation** — generates the next greater lexicographic
  permutation of a range
- **prev_permutation** — generates the next smaller lexicographic
  permutation of a range
- **ranges::shuffle** (C++20) — randomly re-orders elements in a range

---
*Source: https://en.cppreference.com/w/cpp/algorithm/random_shuffle*
