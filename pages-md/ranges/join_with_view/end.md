# std::ranges::join_with_view<V,Pattern>::end

```cpp
constexpr auto end();  // (1) (since C++23)
constexpr auto end() const
  requires ranges::input_range<const V> &&
           ranges::forward_range<const Pattern> &&
           std::is_reference_v<ranges::range_reference_t<const V>>;  // (2) (since C++23)
```

Returns an iterator or a sentinel that compares equal to the end iterator of the
`join_with_view`.

Let `base_` denote the underlying view:

1) Equivalent to: if constexpr (ranges::forward_range<V> &&
   std::is_reference_v<ranges::range_reference_t<V>> &&
   ranges::forward_range<ranges::range_reference_t<V>> &&
   ranges::common_range<V> &&
   ranges::common_range<ranges::range_reference_t<V>>) return
   /*iterator*/<__SimpleView<V> && __SimpleView<Pattern>>{*this,
   ranges::end(base_)}; else return /*sentinel*/<__SimpleView<V> &&
   __SimpleView<Pattern>>{*this};

2) Equivalent to: if constexpr (ranges::forward_range<const V> &&
   ranges::forward_range<ranges::range_reference_t<const V>> &&
   ranges::common_range<const V> &&
   ranges::common_range<ranges::range_reference_t<const V>>) return
   /*iterator*/<true>{*this, ranges::end(base_)}; else return
   /*sentinel*/<true>{*this};

### Parameters

(none)

### Return value

An iterator or sentinel representing the end of the `join_with_view`, as
described above.

### Example

### See also

- **begin (C++23)** — returns an iterator to the beginning (public member
  function)
- **ranges::end (C++20)** — returns a sentinel indicating the end of a range
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/join_with_view/end*
