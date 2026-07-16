# std::ranges::join_with_view<V,Pattern>::base

```cpp
constexpr V base() const& requires std::copy_constructible<V>;  // (1) (since C++23)
constexpr V base() &&;  // (2) (since C++23)
```

Returns a copy of the underlying view.

1) Copy constructs the result from the underlying view.

2) Move constructs the result from the underlying view.

### Parameters

(none)

### Return value

A copy of the underlying view.

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/join_with_view/base*
