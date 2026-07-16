# std::ranges::elements_view<V,F>::*iterator*<Const>::operator[]

```cpp
constexpr decltype(auto) operator[]( difference_type n ) const
    requires ranges::random_access_range<Base>;  // (since C++20)
```

Returns an element at specified relative location, as if by
`/*get-element*/(this->base() + n)`, where for an expression `e`,
`/*get-element*/(e)` is

- `std::get<N>(*e)`, if `ranges::range_reference_t<Base>` is a reference type.
  Otherwise,
- `static_cast<E>(std::get<N>(*e))`, where `E` is
  `std::remove_cv_t<std::tuple_element_t<N, ranges::range_reference_t<Base>>>`.

### Parameters

- **n** — position relative to current location

### Return value

The element at displacement `n` relative to the current location.

### Example

```cpp
#include <iostream>
#include <ranges>
#include <string_view>
#include <tuple>

int main()
{
    using T = std::tuple<int, char, std::string_view>;

    const auto il =
    {
        T{1, 'A', "α"},
        T{2, 'B', "β"},
        T{3, 'C', "γ"},
    };

    std::cout << std::views::elements<0>(il)[1] << ' '   // 2
              << std::views::elements<1>(il)[1] << ' '   // B
              << std::views::elements<2>(il)[1] << '\n'; // β
}
```

Output:

```text
2 B β
```

### See also

- **operator* (C++20)** — accesses the `N`th tuple element (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/elements_view/iterator/operator_at*
