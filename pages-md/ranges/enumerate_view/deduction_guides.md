# deduction guides for `std::ranges::enumerate_view`

```cpp
template< class R >
  enumerate_view( R&& ) -> enumerate_view<views::all_t<R>>;  // (since C++23)
```

The deduction guide is provided for `std::ranges::enumerate_view` to allow
deduction from `range`.

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/enumerate_view/deduction_guides*
