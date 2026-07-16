# std::ranges::view_interface<D>::empty

```cpp
constexpr bool empty()
    requires ranges::sized_range<D> || ranges::forward_range<D>;  // (1) (since C++20)
constexpr bool empty() const
    requires ranges::sized_range<const D> || ranges::forward_range<const D>;  // (2) (since C++20)
```

The default implementation of `empty()` member function checks whether the
object of the derived type's size is `​0​` (if valid), or whether the beginning
iterator and the sentinel compare equal.

1) Let `derived` be a reference bound to `static_cast<D&>(*this)`. Equivalent to
   `return ranges::size(derived) == 0;` if `D` satisfies `sized_range`.
   Otherwise, equivalent to `return ranges::begin(derived) ==
   ranges::end(derived);`.

2) Same as (1), except that `derived` is `static_cast<const D&>(*this)`.

### Parameters

(none)

### Return value

`true` if the size of the object of the derived type is `​0​` (if `D` satisfies
`std::ranges::sized_range`), or its beginning iterator and the sentinel compare
equal, `false` otherwise.

### Notes

Following derived types may use the default implementation of `empty`:

- `std::ranges::common_view`
- `std::ranges::drop_view`
- `std::ranges::drop_while_view`
- `std::ranges::elements_view`
- `std::ranges::filter_view`
- `std::ranges::join_view`
- `std::ranges::lazy_split_view`
- `std::ranges::reverse_view`
- `std::ranges::single_view`
- `std::ranges::split_view`
- `std::ranges::take_view`
- `std::ranges::take_while_view`
- `std::ranges::transform_view`

- `std::ranges::adjacent_transform_view`
- `std::ranges::adjacent_view`
- `std::ranges::as_const_view`
- `std::ranges::as_rvalue_view`
- `std::ranges::cartesian_product_view`
- `std::ranges::chunk_view`
- `std::ranges::chunk_by_view`
- `std::ranges::join_with_view`
- `std::ranges::repeat_view`
- `std::ranges::slide_view`
- `std::ranges::stride_view`
- `std::ranges::zip_view`
- `std::ranges::zip_transform_view`
*(since C++23)*

Although `std::ranges::basic_istream_view` inherits from
`std::ranges::view_interface` and does not declare the `empty()` member
function, it cannot use the default implementation, because it never satisfies
neither `std::ranges::sized_range` nor `std::ranges::forward_range`.

### Example

```cpp
#include <array>
#include <ranges>

int main()
{
    constexpr std::array a{0, 1, 2, 3, 4};
    static_assert(!std::ranges::single_view(a).empty());
    static_assert((a | std::views::take(0)).empty());
    static_assert(!(a | std::views::take(5)).empty());
    static_assert((a | std::views::drop(5)).empty());
    static_assert(!(a | std::views::drop(3)).empty());
    static_assert(std::views::iota(0,0).empty());
    static_assert(!std::views::iota(0).empty());
}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3715 | C++20 | `empty()` only supported `forward_range` types |
      `sized_range`-only types are also supported

### See also

- **empty (C++17)** — checks whether the container is empty (function template)
- **ranges::empty (C++20)** — checks whether a range is empty (customization
  point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/view_interface/empty*
