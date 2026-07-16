# std::ranges::join_view<V>::end

```cpp
constexpr auto end();  // (1) (since C++20)
constexpr auto end() const
    requires ranges::input_range<const V> &&
             std::is_reference_v<ranges::range_reference_t<const V>>;  // (2) (since C++20)
```

Returns a sentinel or an iterator representing the end of the `join_view`.

Let `base_` be the underlying view:

1) Equivalent to if constexpr (ranges::forward_range<V> &&
   std::is_reference_v<ranges::range_reference_t<V>> &&
   ranges::forward_range<ranges::range_reference_t<V>> &&
   ranges::common_range<V> &&
   ranges::common_range<ranges::range_reference_t<V>>) return
   /*iterator*/<__SimpleView<V>>{*this, ranges::end(base_)}; else return
   /*sentinel*/<__SimpleView<V>>{*this};

2) Equivalent to if constexpr (ranges::forward_range<const V> &&
   std::is_reference_v<ranges::range_reference_t<const V>> &&
   ranges::forward_range<ranges::range_reference_t<const V>> &&
   ranges::common_range<const V> &&
   ranges::common_range<ranges::range_reference_t<const V>>) return
   /*iterator*/<true>{*this, ranges::end(base_)}; else return
   /*sentinel*/<true>{*this};

### Parameters

(none)

### Return value

1) sentinel which compares equal to the end iterator.

2) iterator to the element following the last element.

### Example

### See also

- **begin (C++20)** — returns an iterator to the beginning (public member
  function)
- **ranges::end (C++20)** — returns a sentinel indicating the end of a range
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/join_view/end*
