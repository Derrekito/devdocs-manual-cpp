# std::ranges::cartesian_product_view<First, Vs...>::begin

```cpp
constexpr iterator<false> begin()
    requires (!__simple_view<First> || ... || !__simple_view<Vs>);  // (1) (since C++23)
constexpr iterator<true> begin() const
    requires (ranges::range<const First> && ... && ranges::range<const Vs>);  // (2) (since C++23)
```

Returns an iterator to the first element of the `cartesian_product_view`.

Let `bases_` be the tuple of underlying views.

1) Equivalent to `return /*iterator*/<false>(__tuple_transform(ranges::begin,
   bases_));`.

2) Equivalent to `return /*iterator*/<true>(__tuple_transform(ranges::begin,
   bases_));`.

### Parameters

(none)

### Return value

An iterator to the first element.

### Example

```cpp
#include <array>
#include <format>
#include <iostream>
#include <ranges>
#include <string_view>
#include <tuple>
using namespace std::literals;

int main()
{
    constexpr auto a = std::array{"Curiously"sv, "Recurring"sv, "Template"sv, "Pattern"sv};

    constexpr auto v = std::ranges::cartesian_product_view(a[0], a[1], a[2], a[3]);

    constexpr std::tuple<char const&,
                         char const&,
                         char const&,
                         char const&> first{*v.begin()};

    std::cout << std::format("{}{}{}{}{}",
                             std::get<0>(first),
                             std::get<1>(first),
                             std::get<2>(first),
                             std::get<3>(first),
                             '\n');
}
```

Output:

```text
CRTP
```

### See also

- **end (C++23)** — returns an iterator or a sentinel to the end (public member
  function)
- **ranges::begin (C++20)** — returns an iterator to the beginning of a range
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/cartesian_product_view/begin*
