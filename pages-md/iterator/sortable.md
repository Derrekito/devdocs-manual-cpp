# std::sortable

```cpp
template< class I, class Comp = ranges::less, class Proj = std::identity >
concept sortable =
    std::permutable<I> &&
    std::indirect_strict_weak_order<Comp, std::projected<I, Proj>>;  // (since C++20)
```

The `sortable` concept specifies the requirements for algorithms that permute a
range into an ordered range according to `Comp`.

### Semantic requirements

`std::sortable<I, Comp, Proj>` is modeled only if all concepts it subsumes are
modeled.

### See also

- **ranges::sort (C++20)** — sorts a range into ascending order (niebloid)
- **ranges::stable_sort (C++20)** — sorts a range of elements while preserving
  order between equal elements (niebloid)
- **ranges::partial_sort (C++20)** — sorts the first N elements of a range
  (niebloid)
- **ranges::nth_element (C++20)** — partially sorts the given range making sure
  that it is partitioned by the given element (niebloid)
- **ranges::inplace_merge (C++20)** — merges two ordered ranges in-place
  (niebloid)
- **ranges::push_heap (C++20)** — adds an element to a max heap (niebloid)
- **ranges::pop_heap (C++20)** — removes the largest element from a max heap
  (niebloid)
- **ranges::make_heap (C++20)** — creates a max heap out of a range of elements
  (niebloid)
- **ranges::sort_heap (C++20)** — turns a max heap into a range of elements
  sorted in ascending order (niebloid)
- **ranges::next_permutation (C++20)** — generates the next greater
  lexicographic permutation of a range of elements (niebloid)
- **ranges::prev_permutation (C++20)** — generates the next smaller
  lexicographic permutation of a range of elements (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/iterator/sortable*
