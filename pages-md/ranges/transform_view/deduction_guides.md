# deduction guides for `std::ranges::transform_view`

```cpp
template< class R, class F >
transform_view( R&&, F ) -> transform_view<views::all_t<R>, F>;  // (since C++20)
```

The deduction guide is provided for `std::ranges::transform_view` to allow
deduction from `range` and transformation function.

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/transform_view/deduction_guides*
