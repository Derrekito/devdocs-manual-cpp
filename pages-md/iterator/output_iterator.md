# std::output_iterator

```cpp
template< class I, class T >
    concept output_iterator =
        std::input_or_output_iterator<I> &&
        std::indirectly_writable<I, T> &&
        requires(I i, T&& t) {
            *i++ = std::forward<T>(t); /* not required to be equality-preserving */
        };  // (since C++20)
```

The `output_iterator` concept is a refinement of `input_or_output_iterator`,
adding the requirement that it can be used to write values of type and value
category encoded by `T` (via `indirectly_writable`). `equality_comparable` is
not required.

### Semantic requirements

Let `E` be an expression such that `decltype((E))` is `T`, and `i` be a
dereferenceable object of type `I`. std::output_iterator<I, T> is modeled only
if all the concepts it subsumes are modeled, and `*i++ = E;` has effects
equivalent to `*i = E; ++i;`.

### Equality preservation

Expressions declared in requires expressions of the standard library concepts
are required to be equality-preserving (except where stated otherwise).

### Notes

Unlike the LegacyOutputIterator requirements, the `output_iterator` concept does
not require that the iterator category tag be defined.

Algorithms on output iterators should be single pass.

### See also

- **input_or_output_iterator (C++20)** — specifies that objects of a type can be
  incremented and dereferenced (concept)

---
*Source: https://en.cppreference.com/w/cpp/iterator/output_iterator*
