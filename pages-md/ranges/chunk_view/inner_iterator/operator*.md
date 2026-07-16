# std::ranges::chunk_view<V>::*inner-iterator*::operator*

```cpp
constexpr ranges::range_reference_t<V> operator*() const;  // (since C++23)
```

Returns the current element in current chunk in the `chunk_view`.

Let `parent_` be the underlying pointer to `chunk_view`. Equivalent to: `return
**parent_->current_;`.

Before invocation of this operator the expression `*this ==
std::default_sentinel` must be false.

### Parameters

(none)

### Return value

The current element in current chunk.

### Example

### See also

- **base (C++23)** — returns an iterator to the current element (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_view/inner_iterator/operator**
