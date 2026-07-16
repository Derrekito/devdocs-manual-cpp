# std::ranges::take_view<V>::base

```cpp
constexpr V base() const& requires std::copy_constructible<V>;  // (1) (since C++20)
constexpr V base() &&;  // (2) (since C++20)
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
*Source: https://en.cppreference.com/w/cpp/ranges/take_view/base*
