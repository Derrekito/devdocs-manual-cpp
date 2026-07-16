# std::input_iterator

```cpp
template< class I >
    concept input_iterator =
        std::input_or_output_iterator<I> &&
        std::indirectly_readable<I> &&
        requires { typename /*ITER_CONCEPT*/<I>; } &&
        std::derived_from</*ITER_CONCEPT*/<I>, std::input_iterator_tag>;  // (since C++20)
```

The `input_iterator` concept is a refinement of `input_or_output_iterator`,
adding the requirement that the referenced values can be read (via
`indirectly_readable`) and the requirement that the iterator concept tag be
present.

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

### Notes

Unlike the LegacyInputIterator requirements, the `input_iterator` concept does
not require `equality_comparable`, since input iterators are typically compared
with sentinels.

### See also

- **input_or_output_iterator (C++20)** — specifies that objects of a type can be
  incremented and dereferenced (concept)
- **forward_iterator (C++20)** — specifies that an **`input_iterator`** is a
  forward iterator, supporting equality comparison and multi-pass (concept)

---
*Source: https://en.cppreference.com/w/cpp/iterator/input_iterator*
