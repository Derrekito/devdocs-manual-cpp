# std::ranges::join_view<V>::begin

```cpp
constexpr auto begin();  // (1) (since C++20)
constexpr auto begin() const
    requires ranges::input_range<const V> &&
             std::is_reference_v<ranges::range_reference_t<const V>>;  // (2) (since C++20)
```

Returns an iterator to the first element of the `join_view`. Given `base_` is
the underlying view,

1) Equivalent to `return /*iterator*/<true>{*this, ranges::begin(base_)};` if
   `__SimpleView<V>` is satisfied and `ranges::range_reference_t<V>` is
   reference type. Otherwise, equivalent to `return /*iterator*/<false>{*this,
   ranges::begin(base_)};`.

2) Equivalent to `return /*iterator*/<true>{*this, ranges::begin(base_)};`.

### Parameters

(none)

### Return value

Iterator to the first element.

### Notes

When `ranges::range_reference_t<V>` is not a reference type, that is,
deferencing an iterator of `V` returns a prvalue temporary, the `join_view` is
only an `input_range`, in which case only single-pass iteration is supported,
and repeated calls to `begin()` may not give meaningful results.

### Example

### See also

- **end (C++20)** — returns an iterator or a sentinel to the end (public member
  function)
- **ranges::begin (C++20)** — returns an iterator to the beginning of a range
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/join_view/begin*
