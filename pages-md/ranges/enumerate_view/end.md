# std::ranges::enumerate_view<V>::end

```cpp
constexpr auto end() requires (!__SimpleView<V>);  // (1) (since C++23)
constexpr auto end() const requires /*range-with-movable-references*/<const V>;  // (2) (since C++23)
```

Returns an iterator or a sentinel that compares equal to the end iterator of the
`enumerate_view`.

Let `base_` denote the underlying view.

1) Equivalent to: if constexpr (ranges::common_range<V> and
   ranges::sized_range<V>) return /*iterator*/<false>(ranges::end(base_),
   ranges::distance(base_)); else return
   /*sentinel*/<false>(ranges::end(base_));

2) Equivalent to: if constexpr (ranges::common_range<const V> and
   ranges::sized_range<const V>) return /*iterator*/<true>(ranges::end(base_),
   ranges::distance(base_)); else return /*sentinel*/<true>(ranges::end(base_));

### Parameters

(none)

### Return value

An iterator or a sentinel representing the end of the `enumerate_view`, as
described above.

### Example

### See also

- **begin (C++23)** — returns an iterator to the beginning (public member
  function)
- **ranges::end (C++20)** — returns a sentinel indicating the end of a range
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/enumerate_view/end*
