# std::mergeable

```cpp
template< class I1, class I2, class Out, class Comp = ranges::less,
          class Proj1 = std::identity, class Proj2 = std::identity >
concept mergeable =
    std::input_iterator<I1> &&
    std::input_iterator<I2> &&
    std::weakly_incrementable<Out> &&
    std::indirectly_copyable<I1, Out> &&
    std::indirectly_copyable<I2, Out> &&
    std::indirect_strict_weak_order<Comp,
                                    std::projected<I1, Proj1>,
                                    std::projected<I2, Proj2>>;  // (since C++20)
```

The `mergeable` concept specifies the requirements for algorithms that merge two
input ranges into a single output range according to the strict weak ordering
imposed by `Comp`.

### Semantic requirements

`mergeable` is modeled only if all concepts it subsumes are modeled.

### See also

- **ranges::merge (C++20)** — merges two sorted ranges (niebloid)
- **ranges::set_union (C++20)** — computes the union of two sets (niebloid)
- **ranges::set_intersection (C++20)** — computes the intersection of two sets
  (niebloid)
- **ranges::set_difference (C++20)** — computes the difference between two sets
  (niebloid)
- **ranges::set_symmetric_difference (C++20)** — computes the symmetric
  difference between two sets (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/iterator/mergeable*
