# std::indirectly_comparable

```cpp
template< class I1, class I2, class Comp,
          class Proj1 = std::identity, class Proj2 = std::identity >
concept indirectly_comparable =
    std::indirect_binary_predicate<Comp, std::projected<I1, Proj1>, std::projected<I2, Proj2>>;  // (since C++20)
```

The concept `indirectly_comparable` specifies the fundamental algorithm
requirement for comparing values across two independent ranges.

### Semantic requirements

`indirectly_comparable` is modeled only if all concepts it subsumes are modeled.

### See also

- **indirect_binary_predicate (C++20)** — specifies that a callable type, when
  invoked with the result of dereferencing two `indirectly_readable` types,
  satisfies `predicate` (concept)

---
*Source: https://en.cppreference.com/w/cpp/iterator/indirectly_comparable*
