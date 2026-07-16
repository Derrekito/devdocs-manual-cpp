# std::ranges::subrange<I,S,K>::next

```cpp
[[nodiscard]] constexpr subrange next(std::iter_difference_t<I> n = 1) const&
    requires std::forward_iterator<I>;  // (1) (since C++20)
[[nodiscard]] constexpr subrange next(std::iter_difference_t<I> n = 1) &&;  // (2) (since C++20)
```

1) Obtains a `subrange` whose iterator is incremented by `min(n, size())` times
   or decremented by `-n` times respect to that of `*this`, when `n >= 0` or `n
   < 0` respectively. Equivalent to `auto tmp = *this; tmp.advance(n); return
   tmp;`.

2) Increments the stored iterator by `min(n, size())` times or decremented it by
   `-n` times, when `n >= 0` or `n < 0` respectively, and then move-constructs
   the result from `*this`. Equivalent to `advance(n); return
   std::move(*this);`.

The behavior is undefined if:

- `I` does not model `bidirectional_iterator` and `n < 0`, or
- the stored iterator is decremented after becoming a non-decrementable value.

### Parameter

- **n** — number of maximal increments of the iterator

### Return value

A `subrange` whose iterator is incremented by `min(n, size())` times or
decremented by `-n` times respect to the original value of that of `*this`, when
`n >= 0` or `n < 0` respectively.

### Complexity

Generally `min(n, size())` increments or `-n` decrements on the iterator, when
`n >= 0` or `n < 0` respectively.

Constant if `I` models `random_access_iterator`, and either `n < 0` or
`std::sized_sentinel_for<S, I>` is modeled.

### Notes

A call to (2) may leave `*this` in a valid but unspecified state, depending on
the behavior of the move constructor of `I` and `S`.

### Example

### See also

- **prev (C++20)** — obtains a copy of the `subrange` with its iterator
  decremented by a given distance (public member function)
- **advance (C++20)** — advances the iterator by given distance (public member
  function)
- **next (C++11)** — increment an iterator (function template)
- **ranges::next (C++20)** — increment an iterator by a given distance or to a
  bound (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/ranges/subrange/next*
