# std::ranges::chunk_view<V>::*inner-iterator*::base

```cpp
constexpr const ranges::iterator_t<V>& base() const &;  // (since C++23)
```

Returns the underlying cached iterator to current element (in current chunk) in
`chunk_view`.

Let `parent_` be the underlying pointer to the enclosing `chunk_view`.

Equivalent to: `return *parent_->current_;`.

### Parameters

(none)

### Return value

An iterator.

### Example

### See also

- **operator* (C++23)** — accesses the element (public member function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_view/inner_iterator/base*
