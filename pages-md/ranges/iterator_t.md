# std::ranges::iterator_t, std::ranges::const_iterator_t, std::ranges::sentinel_t, std::ranges::const_sentinel_t, std::ranges::range_size_t, std::ranges::range_difference_t, std::ranges::range_value_t, std::ranges::range_reference_t

```cpp
template< class T >
using iterator_t = decltype(ranges::begin(std::declval<T&>()));  // (1) (since C++20)
template< ranges::range R >
using const_iterator_t = decltype(ranges::cbegin(std::declval<R&>()));  // (2) (since C++23)
template< ranges::range R >
using sentinel_t = decltype(ranges::end(std::declval<R&>()));  // (3) (since C++20)
template< ranges::range R >
using const_sentinel_t = decltype(ranges::cend(std::declval<R&>()));  // (4) (since C++23)
template< ranges::sized_range R >
using range_size_t = decltype(ranges::size(std::declval<R&>()));  // (5) (since C++20)
template< ranges::range R >
using range_difference_t = std::iter_difference_t<ranges::iterator_t<R>>;  // (6) (since C++20)
template< ranges::range R >
using range_value_t = std::iter_value_t<ranges::iterator_t<R>>;  // (7) (since C++20)
template< ranges::range R >
using range_reference_t = std::iter_reference_t<ranges::iterator_t<R>>;  // (8) (since C++20)
template< ranges::range R >
using range_const_reference_t =
    std::iter_const_reference_t<ranges::iterator_t<R>>;  // (9) (since C++23)
template< ranges::range R >
using range_rvalue_reference_t =
    std::iter_rvalue_reference_t<ranges::iterator_t<R>>;  // (10) (since C++20)
template< ranges::range R >
using range_common_reference_t =
    std::iter_common_reference_t<ranges::iterator_t<R>>;  // (11) (since C++20)
```

1) Used to obtain the iterator type of the type `T`.

2) Used to obtain the constant iterator type of the `range` type `R`.

3) Used to obtain the sentinel type of the range type `R`.

4) Used to obtain the constant sentinel type of the range type `R`.

5) Used to obtain the size type of the `sized_range` type `R`.

6) Used to obtain the difference type of the iterator type of range type `R`.

7) Used to obtain the value type of the iterator type of range type `R`.

8) Used to obtain the reference type of the iterator type of range type `R`.

9) Used to obtain the constant reference type of the iterator type of range type
   `R`.

10) Used to obtain the rvalue reference type of the iterator type of range type
   `R`.

11) Used to obtain the common reference type of the iterator type of range type
   `R`.

### Template parameters

- **T** — a type that can be used in `std::ranges::begin`
- **R** — a `range` type or a `sized_range` type

### Notes

`iterator_t` can be applied to non-range types, e.g. arrays with unknown bound.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3860 | C++20 | `range_common_reference_t` was missing | added
  LWG 3946 | C++23 | `const_iterator_t` and `const_sentinel_t` were inconsistent
      with the result of `ranges::cbegin` and `ranges::cend` respectively |
      tweaked

### See also

-
  **iter_value_titer_reference_titer_const_reference_titer_difference_titer_rvalue_reference_titer_common_reference_t
  (C++20)(C++20)(C++23)(C++20)(C++20)(C++20)** — computes the associated types
  of an iterator (alias template)

---
*Source: https://en.cppreference.com/w/cpp/ranges/iterator_t*
