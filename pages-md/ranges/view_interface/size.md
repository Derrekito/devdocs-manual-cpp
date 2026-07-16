# std::ranges::view_interface<D>::size

```cpp
constexpr auto size() requires ranges::forward_range<D> &&
    std::sized_sentinel_for<ranges::sentinel_t<D>,
                            ranges::iterator_t<D>>;  // (1) (since C++20)
constexpr auto size() const requires ranges::forward_range<const D> &&
    std::sized_sentinel_for<ranges::sentinel_t<const D>,
                            ranges::iterator_t<const D>>;  // (2) (since C++20)
```

The default implementation of `size()` member function obtains the size of the
range by calculating the difference between the sentinel and the beginning
iterator.

1) Let `derived` be `static_cast<D&>(*this)`. Equivalent to `return
   /*to-unsigned-like*/(ranges::end(derived) - ranges::begin(derived));`, where
   the exposition-only function template `to-unsigned-like` performs the
   explicit cast from the argument type to its corresponding unsigned type.

2) Same as (1), except that `derived` is `static_cast<const D&>(*this)`.

### Parameters

(none)

### Return value

The difference between the sentinel and the beginning iterator of the value of
the derived type, converted to its corresponding unsigned type.

### Notes

Following derived types may use the default implementation of `size()`:

- `std::ranges::drop_while_view`

Following types are derived from `std::ranges::view_interface` and do not
declare their own `size()` member function, but they cannot use the default
implementation, because their iterator and sentinel types never satisfy
`sized_sentinel_for`:

- `std::ranges::basic_istream_view`
- `std::ranges::filter_view`
- `std::ranges::join_view`
- `std::ranges::lazy_split_view`
- `std::ranges::split_view`
- `std::ranges::take_while_view`

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3646 | C++20 | the default implementations of `size` functions returned a
      signed type | they return unsigned type

### See also

- **sizessize (C++17)(C++20)** — returns the size of a container or array
  (function template)
- **ranges::size (C++20)** — returns an integer equal to the size of a range
  (customization point object)
- **ranges::ssize (C++20)** — returns a signed integer equal to the size of a
  range (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/view_interface/size*
