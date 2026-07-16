# deduction guides for `std::ranges::lazy_split_view`

```cpp
template< class R, class P >
lazy_split_view( R&&, P&& )
    -> lazy_split_view<views::all_t<R>, views::all_t<P>>;  // (1) (since C++20)
template< ranges::input_range R >
lazy_split_view( R&&, ranges::range_value_t<R> )
    -> lazy_split_view<views::all_t<R>,
                       ranges::single_view<ranges::range_value_t<R>>>;  // (2) (since C++20)
```

These deduction guides are provided for `lazy_split_view` to allow deduction
from a range and a delimiter.

1) The delimiter is a range of elements.

2) The delimiter is a single element.

### Example

```cpp
#include <ranges>
#include <string_view>
#include <type_traits>
using std::operator""sv;

int main()
{
    std::ranges::lazy_split_view w1{"a::b::c"sv, "::"sv};
    // type of w1 is lazy_split_view<string_view, string_view>:
    static_assert(std::is_same_v<
        decltype(w1),
        std::ranges::lazy_split_view<
            std::basic_string_view<char, std::char_traits<char>>,
            std::basic_string_view<char, std::char_traits<char>>>>);

    std::ranges::lazy_split_view w2{"x,y,z"sv, ','};
    // type of w2 is lazy_split_view<string_view, single_view<char>>:
    static_assert(std::is_same_v<
        decltype(w2),
        std::ranges::lazy_split_view<
            std::basic_string_view<char, std::char_traits<char>>,
            std::ranges::single_view<char>>>);
}
```

---
*Source: https://en.cppreference.com/w/cpp/ranges/lazy_split_view/deduction_guides*
