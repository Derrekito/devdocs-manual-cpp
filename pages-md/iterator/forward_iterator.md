# std::forward_iterator

```cpp
template< class I >
    concept forward_iterator =
        std::input_iterator<I> &&
        std::derived_from</*ITER_CONCEPT*/<I>, std::forward_iterator_tag> &&
        std::incrementable<I> &&
        std::sentinel_for<I, I>;  // (since C++20)
```

This concept refines `std::input_iterator` by requiring that `I` also model
`std::incrementable` (thereby making it suitable for multi-pass algorithms), and
guaranteeing that two iterators to the same range can be compared against each
other.

### Iterator concept determination

Definition of this concept is specified via an exposition-only alias template
`/*ITER_CONCEPT*/`.

In order to determine `/*ITER_CONCEPT*/<I>`, let `ITER_TRAITS<I>` denote `I` if
the specialization `std::iterator_traits<I>` is generated from the primary
template, or `std::iterator_traits<I>` otherwise:

- If `ITER_TRAITS<I>::iterator_concept` is valid and names a type,
  `/*ITER_CONCEPT*/<I>` denotes the type.
- Otherwise, if `ITER_TRAITS<I>::iterator_category` is valid and names a type,
  `/*ITER_CONCEPT*/<I>` denotes the type.
- Otherwise, if `std::iterator_traits<I>` is generated from the primary
  template, `/*ITER_CONCEPT*/<I>` denotes `std::random_access_iterator_tag`.
- Otherwise, `/*ITER_CONCEPT*/<I>` does not denote a type and results in a
  substitution failure.

### Semantic requirements

`I` models `std::forward_iterator` if, and only if `I` models all the concepts
it subsumes, and given objects `i` and `j` of type `I`:

- Comparison between iterators `i` and `j` has a defined result if
- Pointers and references obtained from a forward iterator into a range remain
  valid while the range exists.
- If `i` and `j` are dereferenceable, they offer the *multi-pass guarantee*,
  that is:

### Notes

Unlike the LegacyForwardIterator requirements, the `forward_iterator` concept
does not require dereference to return a reference.

### See also

- **input_iterator (C++20)** — specifies that a type is an input iterator, that
  is, its referenced values can be read and it can be both pre- and
  post-incremented (concept)
- **bidirectional_iterator (C++20)** — specifies that a **`forward_iterator`**
  is a bidirectional iterator, supporting movement backwards (concept)

---
*Source: https://en.cppreference.com/w/cpp/iterator/forward_iterator*
