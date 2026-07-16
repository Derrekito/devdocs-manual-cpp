# std::sample

Selects `n` elements from a range without replacement, such that every
possible sample of that size has equal probability, and writes them to
an output iterator. If `n` exceeds the population size, the whole
population is selected. (C++17)

```cpp skip
std::sample(first, last, out, n, g);   // (since C++17)
```

### What you provide

- **first, last** — the population to sample from (LegacyInputIterator
  at minimum).
- **out** — output iterator the samples are written to
  (LegacyOutputIterator); must also be LegacyRandomAccessIterator if
  `first`/`last` don't meet LegacyForwardIterator. Must not point into
  `[first, last)` — that's undefined behavior.
- **n** — number of samples to draw (an integer type).
- **g** — a UniformRandomBitGenerator source of randomness, whose result
  type converts to `n`'s type.

### Guarantees and costs

- Linear in the distance between `first` and `last`.
- Every possible sample of size `n` is equally likely to be produced.
- Stable — keeps the selected elements' relative order from the
  population — only when `first`/`last` meet LegacyForwardIterator; with
  plain input iterators, order isn't preserved.
- May be implemented as either reservoir sampling or selection sampling;
  the standard doesn't mandate which.

### Gotchas

- With only input iterators for the population, don't expect the sample
  to preserve original order — use forward iterators (or better) if
  order matters.
- An output iterator that aliases the population range is undefined
  behavior.
- Like all `<random>`-based algorithms, results aren't portable across
  standard library implementations, even with an identical seed.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <random>
#include <string>

int main()
{
    std::string in{"ABCDEFGHIJK"};
    std::string out;

    std::mt19937 g(12345);   // fixed seed so this check is reproducible
    std::sample(in.begin(), in.end(), std::back_inserter(out), 4, g);

    bool all_from_in = std::all_of(out.begin(), out.end(), [&](char c)
    {
        return in.find(c) != std::string::npos;
    });
    std::cout << out.size() << " letters sampled, all from input: "
              << std::boolalpha << all_from_in << '\n';
}
```

```text
4 letters sampled, all from input: true
```

### Reference

Full declaration:

```cpp skip
template< class PopulationIterator, class SampleIterator,
          class Distance, class URBG >
SampleIterator sample( PopulationIterator first, PopulationIterator last,
                       SampleIterator out, Distance n,
                       URBG&& g );  // (since C++17)
```

`PopulationIterator` must meet LegacyInputIterator; `SampleIterator` must
meet LegacyOutputIterator (and LegacyRandomAccessIterator if
`PopulationIterator` isn't at least LegacyForwardIterator);
`PopulationIterator`'s value type must be writable to `out`; `Distance`
must be an integer type; `std::remove_reference_t<URBG>` must meet
UniformRandomBitGenerator with a result type convertible to `Distance`.
Returns a copy of `out` positioned past the last sample written. Feature-
test macro `__cpp_lib_sample` (value `201603L`) signals availability.

### See also

- **shuffle** (C++11) — randomly re-orders every element of a range
- **ranges::sample** (C++20) — constrained version with the same
  selection guarantees

---
*Source: https://en.cppreference.com/w/cpp/algorithm/sample*
