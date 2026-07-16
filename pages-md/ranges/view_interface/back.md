# std::ranges::view_interface<D>::back

```cpp
constexpr decltype(auto) back()
    requires ranges::bidirectional_range<D> && ranges::common_range<D>;  // (1) (since C++20)
constexpr decltype(auto) back() const
    requires ranges::bidirectional_range<const D> && ranges::common_range<const D>;  // (2) (since C++20)
```

The default implementation of `back()` member function returns the last element
in the view of the derived type. Whether the element is returned by value or by
reference depends on the `operator*` of the iterator type.

1) Let `derived` be `static_cast<D&>(*this)`. Equivalent to `return
   *ranges::prev(ranges::end(derived));`. The behavior is undefined if `empty()`
   is `true` (i.e. the beginning iterator compares equal to the sentinel), even
   if the iterator obtained in the same way is dereferenceable.

2) Same as (1), except that `derived` is `static_cast<const D&>(*this)`.

### Parameters

(none)

### Return value

The last element in the view.

### Notes

In C++20, no type derived from `std::ranges::view_interface` in the standard
library provides their own `back()` member function.

However, following derived types cannot use the default implementations, as they
never satisfy neither `bidirectional_range` nor `common_range`:

- `std::ranges::basic_istream_view`
- `std::ranges::lazy_split_view`
- `std::ranges::split_view`
- `std::ranges::take_while_view`

The inherited `back()` member function is available for
`std::ranges::empty_view`, but a call to it always results in undefined
behavior.

### Example

### See also

- **rbegincrbegin (C++14)** — returns a reverse iterator to the beginning of a
  container or array (function template)
- **ranges::rbegin (C++20)** — returns a reverse iterator to a range
  (customization point object)
- **ranges::crbegin (C++20)** — returns a reverse iterator to a read-only range
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/view_interface/back*
