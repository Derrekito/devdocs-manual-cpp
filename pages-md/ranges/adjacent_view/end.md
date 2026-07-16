# std::ranges::adjacent_view<V,N>::end

```cpp
constexpr auto end() requires (!__SimpleView<V>);  // (1) (since C++23)
constexpr auto end() const requires ranges::range<const V>;  // (2) (since C++23)
```

Returns an iterator or a sentinel representing the end of the `adjacent_view`.

Let `base_` be the underlying view.

1) Equivalent to: if constexpr (ranges::common_range<V>) return
   /*iterator*/<false>(__as_sentinel{}, ranges::begin(base_),
   ranges::end(base_)); else return /*sentinel*/<false>(ranges::end(base_));

2) Equivalent to: if constexpr (ranges::common_range<const V>) return
   /*iterator*/<true>(__as_sentinel{}, ranges::begin(base_),
   ranges::end(base_)); else return /*sentinel*/<true>(ranges::end(base_));

### Parameters

(none)

### Return value

An iterator to the element following the last element, if the underlying view
`V` models `common_range`. Otherwise, a sentinel which compares equal to the end
iterator.

### Notes

`ranges::adjacent_view<V,N>` models `common_range` whenever the underlying view
`V` does.

### Example

### See also

- **begin (C++23)** — returns an iterator to the beginning (public member
  function)
- **ranges::end (C++20)** — returns a sentinel indicating the end of a range
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/adjacent_view/end*
