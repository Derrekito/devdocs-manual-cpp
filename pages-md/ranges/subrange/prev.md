# std::ranges::subrange<I,S,K>::prev

```cpp
[[nodiscard]] constexpr subrange prev( std::iter_difference_t<I> n = 1 ) const
    requires std::bidirectional_iterator<I>;  // (since C++20)
```

Obtains a `subrange` whose iterator is decremented by `n` times or incremented
by `min(-n, size())` times respect to that of `*this`, when `n >= 0` or `n < 0`
respectively.

Equivalent to `auto tmp = *this; tmp.advance(-n); return tmp;`. The behavior is
undefined if the iterator is decremented after being a non-decrementable value.

### Parameters

- **n** — number of minimal decrements of the iterator

### Return value

A `subrange` whose iterator is decremented by `n` times or incremented by
`min(-n, size())` times respect to that of `*this`, when `n >= 0` or `n < 0`
respectively.

### Complexity

Generally `n` decrements or `min(-n, size())` increments on the iterator, when
`n >= 0` or `n < 0` respectively.

Constant if `I` models `random_access_iterator`, and either `n >= 0` or
`std::sized_sentinel_for<S, I>` is modeled.

### Example

### See also

- **next (C++20)** — obtains a copy of the `subrange` with its iterator advanced
  by a given distance (public member function)
- **advance (C++20)** — advances the iterator by given distance (public member
  function)
- **prev (C++11)** — decrement an iterator (function template)
- **ranges::prev (C++20)** — decrement an iterator by a given distance or to a
  bound (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/ranges/subrange/prev*
