# std::input_or_output_iterator

```cpp
template< class I >
    concept input_or_output_iterator =
        requires(I i) {
            { *i } -> /*can-reference*/;
        } &&
        std::weakly_incrementable<I>;  // (since C++20)
```

The `input_or_output_iterator` concept forms the basis of the iterator concept
taxonomy; every iterator type satisfies the `input_or_output_iterator`
requirements.

The exposition-only concept `/*can-reference*/` is satisfied if and only if the
type is referenceable (in particular, not `void`).

### Notes

A typical `input_or_output_iterator` class only needs to be `movable`, and
provide the following members:

- Member typedef `difference_type` (used by `std::iter_difference_t`).
- Prefix `operator++` that returns a reference to `*this`.
- Postfix `operator++`.
- The dereference operator `operator*`.

Alternatively, the `difference_type` can be provided by specializing either
`std::iterator_traits` or `std::incrementable_traits`. The functions can be
defined as non-members, to be found by argument-dependent lookup.

`input_or_output_iterator` itself only specifies operations for dereferencing
and incrementing an iterator. Most algorithms will require additional
operations, for example:

- comparing iterators with sentinels (see `sentinel_for`);
- reading values from an iterator (see `indirectly_readable` and
  `input_iterator`);
- writing values to an iterator (see `indirectly_writable` and
  `output_iterator`);
- a richer set of iterator movements (see `forward_iterator`,
  `bidirectional_iterator`, `random_access_iterator`).

Unlike the LegacyIterator requirements, the `input_or_output_iterator` concept
does not require copyability.

---
*Source: https://en.cppreference.com/w/cpp/iterator/input_or_output_iterator*
