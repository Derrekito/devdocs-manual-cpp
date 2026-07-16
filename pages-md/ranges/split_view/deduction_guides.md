# deduction guides for `std::ranges::split_view`

```cpp
template< class R, class P >
split_view( R&&, P&& )
    -> split_view<views::all_t<R>, views::all_t<P>>;  // (1) (since C++20)
template< ranges::input_range R >
split_view( R&&, ranges::range_value_t<R> )
    -> split_view<views::all_t<R>, ranges::single_view<ranges::range_value_t<R>>>;  // (2) (since C++20)
```

These deduction guides are provided for `split_view` to allow deduction from a
range and a delimiter.

1) The delimiter is a range of elements.

2) The delimiter is a single element.

### Example

```cpp
#include <ranges>
#include <string_view>
#include <type_traits>
using std::operator""sv;

int main() {
    std::ranges::split_view w1{"a::b::c"sv, "::"sv};
    static_assert(std::is_same_v<
        decltype(w1),
        std::ranges::split_view<std::string_view, std::string_view>>);

    std::ranges::split_view w2{"x,y,z"sv, ','};
    static_assert(std::is_same_v<
        decltype(w2),
        std::ranges::split_view<std::string_view, std::ranges::single_view<char>>>);
}
```

---
*Source: https://en.cppreference.com/w/cpp/ranges/split_view/deduction_guides*
