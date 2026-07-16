# std::ranges::chunk_view<V>::*iterator*<Const>::base

```cpp
constexpr ranges::iterator_t<Base> base() const;  // (since C++23)
```

Returns the underlying iterator to the begin of current chunk.

Equivalent to `return current_;`.

### Parameters

(none)

### Return value

An iterator.

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_view/iterator/base*
