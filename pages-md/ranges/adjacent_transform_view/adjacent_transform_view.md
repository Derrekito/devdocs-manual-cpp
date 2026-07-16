# std::ranges::adjacent_transform_view<V,F,N>::adjacent_transform_view

```cpp
adjacent_transform_view() = default;  // (1) (since C++23)
constexpr explicit adjacent_transform_view( V base, F fun );  // (2) (since C++23)
```

Constructs an `adjacent_transform_view`.

1) Default constructor. Default-initializes the underlying data members `fun_`
   and `inner_`.

2) Move constructs the underlying data members: `fun_` with `std::move(fun)` and
   `inner_` with `std::move(base)`.

### Parameters

- **base** — the underlying view
- **fun** — the N-ary transformation function

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <ranges>

int main()
{
    constexpr static auto v = {1, 2, 3, 4, 5};
    constexpr auto mul = [](auto... x) { return (... * x); };
    constexpr auto view = std::views::adjacent_transform<3>(v, mul);
    std::ranges::copy(view, std::ostream_iterator<int>(std::cout, " "));
    std::cout << '\n';
}
```

Output:

```text
6 24 60
```

---
*Source: https://en.cppreference.com/w/cpp/ranges/adjacent_transform_view/adjacent_transform_view*
