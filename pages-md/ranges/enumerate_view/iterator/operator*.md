# std::ranges::enumerate_view<V>::*iterator*<Const>::operator*

```cpp
constexpr auto operator*() const;  // (since C++23)
```

Returns the current element in the `enumerate_view`.

Equivalent to: `return reference-type(pos_, *current_);`.

### Parameters

(none)

### Return value

The current element.

### Example

### See also

- **operator[] (C++23)** — accesses an element by index (public member function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/enumerate_view/iterator/operator**
