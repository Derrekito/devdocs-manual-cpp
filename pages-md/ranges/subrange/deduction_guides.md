# deduction guides for `std::ranges::subrange`

```cpp
template< std::input_or_output_iterator I, std::sentinel_for<I> S >
subrange(I, S) -> subrange<I, S>;  // (1) (since C++20)
template< std::input_or_output_iterator I, std::sentinel_for<I> S >
subrange(I, S, /*make-unsigned-like-t*/<std::iter_difference_t<I>>) ->
    subrange<I, S, ranges::subrange_kind::sized>;  // (2) (since C++20)
template< ranges::borrowed_range<R> >
subrange(R&&) ->
    subrange<ranges::iterator_t<R>, ranges::sentinel_t<R>,
             (ranges::sized_range<R> ||
              std::sized_sentinel_for<ranges::sentinel_t<R>,
              ranges::iterator_t<R>>) ?
             ranges::subrange_kind::sized : ranges::subrange_kind::unsized>;  // (3) (since C++20)
template< ranges::borrowed_range<R> >
subrange(R&&, /*make-unsigned-like-t*/<ranges::range_difference_t<R>>) ->
    subrange<ranges::iterator_t<R>, ranges::sentinel_t<R>,
             ranges::subrange_kind::sized>;  // (4) (since C++20)
```

These deduction guides are provided for `std::ranges::subrange`.

1) Deduces the template arguments from the type of iterator and sentinel. The
   `subrange` is sized if `std::sized_sentinel_for<S, I>` is satisfied, as
   determined by the default template argument.

2) Deduces the template arguments from the type of iterator and sentinel, while
   the size of range is specified. The `subrange` is always sized.

3) Deduces the template arguments from the type of range. The `subrange` is
   sized if the size can be obtained from the range or its iterator and
   sentinel.

4) Deduces the template arguments from the type of range, while the size of
   range is specified. The `subrange` is always sized.

The exposition-only alias template `make-unsigned-like-t` maps each integer-like
type to its corresponding unsigned version.

### Notes

While constructing the `subrange` object,

- for (1,2), the behavior is undefined if the iterator-sentinel pair does not
  denote a valid range,
- for (2,4), the behavior is undefined if the given size is not equal to the
  size of the range.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3404 | C++20 | meaningless deduction guides from pair-like types were
      provided | removed

---
*Source: https://en.cppreference.com/w/cpp/ranges/subrange/deduction_guides*
