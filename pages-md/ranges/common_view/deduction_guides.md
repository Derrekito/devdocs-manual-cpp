# deduction guides for `std::ranges::common_view`

```cpp
template< class R >
common_view( R&& ) -> common_view<views::all_t<R>>;  // (since C++20)
```

The deduction guide is provided for `std::ranges::common_view` to allow
deduction from `range`.

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/common_view/deduction_guides*
