# std::ranges::view_interface<D>::front

```cpp
constexpr decltype(auto) front()
    requires ranges::forward_range<D>;  // (1) (since C++20)
constexpr decltype(auto) front() const
    requires ranges::forward_range<const D>;  // (2) (since C++20)
```

The default implementation of `front()` member function returns the first
element in the view of the derived type. Whether the element is returned by
value or by reference depends on the `operator*` of the iterator type.

1) Let `derived` be `static_cast<D&>(*this)`. Equivalent to `return
   *ranges::begin(derived);`. The behavior is undefined if `empty()` is `true`
   (i.e. the beginning iterator compares equal to the sentinel), even if the
   iterator obtained in the same way is dereferenceable.

2) Same as (1), except that `derived` is `static_cast<const D&>(*this)`.

### Parameters

(none)

### Return value

The first element in the view.

### Notes

In C++20, no type derived from `std::ranges::view_interface` in the standard
library provides their own `front()` member function. Almost all of these types
use the default implementation.

A notable exception is `std::ranges::basic_istream_view`. For it never satisfies
`forward_range`, the view cannot use the inherited `front()`.

The inherited `front()` member function is available for
`std::ranges::empty_view`, but a call to it always results in undefined
behavior.

### Example

### See also

- **begincbegin (C++11)(C++14)** — returns an iterator to the beginning of a
  container or array (function template)
- **ranges::begin (C++20)** — returns an iterator to the beginning of a range
  (customization point object)
- **ranges::cbegin (C++20)** — returns an iterator to the beginning of a
  read-only range (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/view_interface/front*
