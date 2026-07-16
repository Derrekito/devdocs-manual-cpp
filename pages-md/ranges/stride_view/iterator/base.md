# std::ranges::stride_view<V>::*iterator*<Const>::base

```cpp
constexpr ranges::iterator_t<Base> base() &&;  // (1) (since C++23)
constexpr const ranges::iterator_t<Base>& base() const& noexcept;  // (2) (since C++23)
```

Returns the underlying iterator. Let `current_` be the underlying iterator.

1) Equivalent to: `return std::move(current_);`.

2) Equivalent to: `return current_;`.

### Parameters

(none)

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/stride_view/iterator/base*
