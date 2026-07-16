# std::ranges::chunk_view<V>::*iterator*<Const>::operator[]

```cpp
constexpr value_type operator[]( difference_type pos ) const
    requires ranges::random_access_range<Base>;  // (since C++23)
```

Returns an element at specified relative location.

Equivalent to: `return *(*this + pos);`.

### Parameters

- **pos** — position relative to current location

### Return value

The element (of type `value_type`) at displacement `pos` relative to the current
location.

### Example

### See also

- **operator* (C++23)** — accesses the element (public member function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_view/iterator/operator_at*
