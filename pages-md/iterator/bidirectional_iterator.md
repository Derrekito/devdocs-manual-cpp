# std::bidirectional_iterator

```cpp
template< class I >
    concept bidirectional_iterator =
        std::forward_iterator<I> &&
        std::derived_from</*ITER_CONCEPT*/<I>, std::bidirectional_iterator_tag> &&
        requires(I i) {
            { --i } -> std::same_as<I&>;
            { i-- } -> std::same_as<I>;
        };  // (since C++20)
```

The concept `bidirectional_iterator` refines `forward_iterator` by adding the
ability to move an iterator backward.

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

A bidirectional iterator `r` is said to be *decrementable* if and only if there
exists some `s` such that `++s == r`.

std::bidirectional_iterator<I> is modeled only if all the concepts it subsumes
are modeled, and given two objects `a` and `b` of type `I`:

- If `a` is decrementable, `a` is in the domain of the expressions `--a` and
  `a--`.
- Pre-decrement yields an lvalue that refers to the operand:
  `std::addressof(--a) == std::addressof(a)`.
- Post-decrement yields the previous value of the operand: if `bool(a == b)`,
  then `bool(a-- == b)`.
- Post-decrement and pre-decrement perform the same modification on its operand:
  If `bool(a == b)`, then after evaluating both `a--` and `--b`, `bool(a == b)`
  still holds.
- Increment and decrement are inverses of each other:

### Equality preservation

Expressions declared in requires expressions of the standard library concepts
are required to be equality-preserving (except where stated otherwise).

### Notes

Unlike the LegacyBidirectionalIterator requirements, the
`bidirectional_iterator` concept does not require dereference to return an
lvalue.

### See also

- **forward_iterator (C++20)** — specifies that an `input_iterator` is a forward
  iterator, supporting equality comparison and multi-pass (concept)
- **random_access_iterator (C++20)** — specifies that a
  **`bidirectional_iterator`** is a random-access iterator, supporting
  advancement in constant time and subscripting (concept)

---
*Source: https://en.cppreference.com/w/cpp/iterator/bidirectional_iterator*
