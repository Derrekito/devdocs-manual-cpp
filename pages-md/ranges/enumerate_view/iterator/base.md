# std::ranges::enumerate_view<V>::*iterator*<Const>::base

```cpp
constexpr const ranges::iterator_t<Base>& base() const& noexcept;  // (1) (since C++23)
constexpr ranges::iterator_t<Base> base() &&;  // (2) (since C++23)
```

Returns the underlying iterator. Let `current_` be the underlying iterator.

1) Equivalent to: `return current_;`.

2) Equivalent to: `return std::move(current_);`.

### Parameters

(none)

### Return value

An iterator to the current element in `enumerate_view`.

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/enumerate_view/iterator/base*
