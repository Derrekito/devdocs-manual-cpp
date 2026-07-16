# std::ranges::view_interface<D>::operator[]

```cpp
template<ranges::random_access_range R = D>
constexpr decltype(auto) operator[]( ranges::range_difference_t<R> n );  // (1) (since C++20)
template<ranges::random_access_range R = const D>
constexpr decltype(auto) operator[]( ranges::range_difference_t<R> n ) const;  // (2) (since C++20)
```

The default implementation of `operator[]` member function obtains the element
at the specified offset relative to the beginning iterator, reusing the
`operator[]` of the iterator type.

1) Let `derived` be `static_cast<D&>(*this)`. Equivalent to `return
   ranges::begin(derived)[n];`.

2) Same as (1), except that `derived` is `static_cast<const D&>(*this)`.

### Parameters

- **n** — position of the element to return

### Return value

The element at offset `n` relative to the beginning iterator.

### Notes

In C++20, no type derived from `std::ranges::view_interface` in the standard
library provides their own `operator[]` member function.

However, following derived types cannot use the default implementations, as they
never satisfy `random_access_range`:

- `std::ranges::basic_istream_view`
- `std::ranges::filter_view`
- `std::ranges::join_view`
- `std::ranges::lazy_split_view`
- `std::ranges::split_view`

The inherited `operator[]` member function is available for
`std::ranges::empty_view`, but a call to it always results in undefined
behavior.

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/view_interface/operator_at*
