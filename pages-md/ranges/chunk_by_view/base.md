# std::ranges::chunk_by_view<V,Pred>::base

```cpp
constexpr V base() const& requires std::copy_constructible<V>;  // (1) (since C++23)
constexpr V base() &&;  // (2) (since C++23)
```

Returns a copy of the underlying view `base_`.

1) Copy constructs the result from the underlying view. Equivalent to: `return
   base_;`

2) Move constructs the result from the underlying view. Equivalent to: `return
   std::move(base_);`

### Parameters

(none)

### Return value

A copy of the underlying view.

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_by_view/base*
