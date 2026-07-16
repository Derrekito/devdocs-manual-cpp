# deduction guides for `std::ranges::drop_view`

```cpp
template< class R >
drop_view( R&&, ranges::range_difference_t<R> ) -> drop_view<views::all_t<R>>;  // (since C++20)
```

The deduction guide is provided for `std::ranges::drop_view` to allow deduction
from `range` and number of elements.

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/drop_view/deduction_guides*
