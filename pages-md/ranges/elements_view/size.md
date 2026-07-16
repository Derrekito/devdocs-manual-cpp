# std::ranges::elements_view<V,N>::size

```cpp
constexpr auto size() requires ranges::sized_range<V>;  // (since C++20)
constexpr auto size() const requires ranges::sized_range<const V>;  // (since C++20)
```

Returns the number of elements, i.e. `ranges::size(base_)`, where `base_` is the
underlying view.

### Parameters

(none)

### Return value

The number of elements.

### Example

### See also

- **ranges::size (C++20)** — returns an integer equal to the size of a range
  (customization point object)
- **ranges::ssize (C++20)** — returns a signed integer equal to the size of a
  range (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/elements_view/size*
