# *no-throw-input-iterator*, *no-throw-forward-iterator*, *no-throw-sentinel-for*, *no-throw-input-range*, *no-throw-forward-range*

```cpp
template< class I >
concept no-throw-input-iterator =
    std::input_iterator<I> &&
    std::is_lvalue_reference_v<std::iter_reference_t<I>> &&
    std::same_as<std::remove_cvref_t<std::iter_reference_t<I>>, std::iter_value_t<I>>;  // (1) (exposition only*)
template< class I >
concept no-throw-forward-iterator =
    no-throw-input-iterator<I> &&
    std::forward_iterator<I> &&
    no-throw-sentinel-for<I, I>;  // (2) (exposition only*)
template< class S, class I >
concept no-throw-sentinel-for = std::sentinel_for<S, I>;  // (3) (exposition only*)
template< class R >
concept no-throw-input-range =
    ranges::range<R> &&
    no-throw-input-iterator<ranges::iterator_t<R>> &&
    no-throw-sentinel-for<ranges::sentinel_t<R>, ranges::iterator_t<R>>;  // (4) (exposition only*)
template< class R >
concept no-throw-forward-range =
    no-throw-input-range<R> &&
    no-throw-forward-iterator<ranges::iterator_t<R>>;  // (5) (exposition only*)
```

These exposition-only concepts specify that no exceptions are thrown from
operations required by algorithms on iterators, sentinels, and ranges.

1) The `no-throw-input-iterator` concept requires that dereferencing the
   iterator yields an lvalue, like `contiguous_iterator` and
   LegacyForwardIterator.

### Semantic requirements

Like all standard concepts, every concept listed here is modeled only if all
concepts it subsumes are modeled.

1) A type `I` models `no-throw-input-iterator` only if no exceptions are thrown
   from increment, copy construction, move construction, copy assignment, move
   assignment, or indirection through valid iterators.

3) Types `S` and `I` model `no-throw-sentinel-for` only if no exceptions are
   thrown from copy construction, move construction, copy assignment, move
   assignment, or comparisons between valid values of type `I` and `S`.

4) A type `R` models `no-throw-input-range` only if no exceptions are thrown
   from calls to `ranges::begin` and `ranges::end` on an object of type `R`.

### Notes

These concepts allow some operations on iterators and sentinels to throw
exceptions, e.g. operations on invalid values.

### See also

- **input_iterator (C++20)** — specifies that a type is an input iterator, that
  is, its referenced values can be read and it can be both pre- and
  post-incremented (concept)
- **forward_iterator (C++20)** — specifies that an `input_iterator` is a forward
  iterator, supporting equality comparison and multi-pass (concept)
- **sentinel_for (C++20)** — specifies a type is a sentinel for an
  `input_or_output_iterator` type (concept)
- **ranges::input_range (C++20)** — specifies a range whose iterator type
  satisfies `input_iterator` (concept)
- **ranges::forward_range (C++20)** — specifies a range whose iterator type
  satisfies `forward_iterator` (concept)

---
*Source: https://en.cppreference.com/w/cpp/memory/ranges/nothrow_concepts*
