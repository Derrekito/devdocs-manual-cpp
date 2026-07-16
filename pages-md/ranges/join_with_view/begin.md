# std::ranges::join_with_view<V,Pattern>::begin

```cpp
constexpr auto begin();  // (1) (since C++23)
constexpr auto begin() const
  requires ranges::input_range<const V> &&
           ranges::forward_range<const Pattern> &&
           std::is_reference_v<ranges::range_reference_t<const V>>;  // (2) (since C++23)
```

Returns an iterator to the first element of the `join_with_view`.

Let `base_` denote the underlying view:

1) Equivalent to `return /*iterator*/<true>{*this, ranges::begin(base_)};` if
   `V` and `Pattern` each model `__SimpleView` and
   `ranges::range_reference_t<V>` is a reference type; otherwise equivalent to
   `return /*iterator*/<false>{*this, ranges::begin(base_)};`.

2) Equivalent to `return /*iterator*/<true>{*this, ranges::begin(base_)};`.

### Parameters

(none)

### Return value

An iterator to the first element of the `join_with_view`, as described above.

### Example

### See also

- **end (C++23)** — returns an iterator or a sentinel to the end (public member
  function)
- **ranges::begin (C++20)** — returns an iterator to the beginning of a range
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/join_with_view/begin*
