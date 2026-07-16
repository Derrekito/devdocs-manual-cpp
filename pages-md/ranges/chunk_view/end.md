# std::ranges::chunk_view<V>::end

```cpp
V models only input_range
constexpr std::default_sentinel_t end() const noexcept;  // (1) (since C++23)
V models forward_range
constexpr auto end() requires (!__simple_view<V>);  // (2) (since C++23)
constexpr auto end() const requires ranges::forward_range<const V>;  // (3) (since C++23)
```

Returns an iterator or a `std::default_sentinel` that compares equal to the end
iterator of the `chunk_view`.

1) Available only if `V` models `input_range`. Equivalent to: `return
   std::default_sentinel`.

2,3) Available if `V` models `forward_range`. Let `base_` denote the underlying
   adapted view, `n_` denote the stored chunk size, and `iterator` denote the
   nested iterator class.

2) Equivalent to: if constexpr (ranges::common_range<V> &&
   ranges::sized_range<V>) { auto missing = (n_ - ranges::distance(base_) % n_)
   % n_; return iterator<false>(this, ranges::end(base_), missing); } else if
   constexpr (ranges::common_range<V> && !ranges::bidirectional_range<V>) return
   iterator<false>(this, ranges::end(base_)); else return std::default_sentinel;

3) Equivalent to: if constexpr (ranges::common_range<const V> &&
   ranges::sized_range<const V>) { auto missing = (n_ - ranges::distance(base_)
   % n_) % n_; return iterator<true>(this, ranges::end(base_), missing); } else
   if constexpr (ranges::common_range<const V> &&
   !ranges::bidirectional_range<const V>) return iterator<true>(this,
   ranges::end(base_)); else return std::default_sentinel;

### Parameters

(none)

### Return value

An iterator or sentinel representing the end of the `chunk_view`, as described
above.

### Example

### See also

- **begin (C++23)** — returns an iterator to the beginning (public member
  function)
- **ranges::end (C++20)** — returns a sentinel indicating the end of a range
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_view/end*
