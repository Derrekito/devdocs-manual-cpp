# std::ranges::chunk_view<V>::*outer-iterator*::operator*

```cpp
constexpr value_type operator*() const;  // (since C++23)
```

Returns the current chunk in the `chunk_view`.

Let `parent_` be the underlying pointer, and `value_type` be a nested class that
is the value type of the iterator.

Equivalent to: `return value_type(*parent_);`.

Before invocation of this operator the expression `*this ==
std::default_sentinel` must be false.

### Parameters

(none)

### Return value

The current element (a chunk).

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_view/outer_iterator/operator**
