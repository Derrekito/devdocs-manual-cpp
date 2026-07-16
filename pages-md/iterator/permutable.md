# std::permutable

```cpp
template< class I >
concept permutable =
    std::forward_iterator<I> &&
    std::indirectly_movable_storable<I, I> &&
    std::indirectly_swappable<I, I>;  // (since C++20)
```

The concept `permutable` refines `std::forward_iterator` by adding requirements
for reordering through moves and swaps.

### Semantic requirements

`I` models `permutable` only if all concepts it subsumes are modeled.

### See also

- **sortable (C++20)** — specifies the common requirements of algorithms that
  permute sequences into ordered sequences (concept)
- **ranges::removeranges::remove_if (C++20)(C++20)** — removes elements
  satisfying specific criteria (niebloid)
- **ranges::unique (C++20)** — removes consecutive duplicate elements in a range
  (niebloid)
- **ranges::reverse (C++20)** — reverses the order of elements in a range
  (niebloid)
- **ranges::rotate (C++20)** — rotates the order of elements in a range
  (niebloid)
- **ranges::shuffle (C++20)** — randomly re-orders elements in a range
  (niebloid)
- **ranges::partition (C++20)** — divides a range of elements into two groups
  (niebloid)
- **ranges::stable_partition (C++20)** — divides elements into two groups while
  preserving their relative order (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/iterator/permutable*
