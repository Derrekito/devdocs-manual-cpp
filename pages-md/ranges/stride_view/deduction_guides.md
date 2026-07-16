# deduction guides for `std::ranges::stride_view`

```cpp
template< class R >
stride_view( R&&, ranges::range_difference_t<R> ) -> stride_view<views::all_t<R>>;  // (since C++23)
```

The deduction guide is provided for `std::ranges::stride_view` to allow
deduction from `range` and number of elements.

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/stride_view/deduction_guides*
