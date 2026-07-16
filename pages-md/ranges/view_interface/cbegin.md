# std::ranges::view_interface<D>::cbegin

```cpp
constexpr auto cbegin();  // (1) (since C++23)
constexpr auto cbegin() const requires ranges::range<const D>;  // (2) (since C++23)
```

The default implementation of `cbegin()` member function returns a constant
beginning iterator of the range.

1) Let `derived` be a reference bound to `static_cast<D&>(*this)`.

Equivalent to `return ranges::cbegin(derived);`.

2) Same as (1), except that `derived` is `static_cast<const D&>(*this)`.

### Parameters

(none)

### Return value

A constant beginning iterator of the range.

### Notes

All range adaptors and range factories in the standard library and
`std::ranges::subrange` use the default implementation of `cbegin`.

### Example

### See also

- **begincbegin (C++11)(C++14)** — returns an iterator to the beginning of a
  container or array (function template)
- **ranges::cbegin (C++20)** — returns an iterator to the beginning of a
  read-only range (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/view_interface/cbegin*
