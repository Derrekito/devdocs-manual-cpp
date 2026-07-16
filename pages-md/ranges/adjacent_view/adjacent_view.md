# std::ranges::adjacent_view<V,N>::adjacent_view

```cpp
adjacent_view() requires std::default_initializable<V> = default;  // (1) (since C++23)
constexpr explicit adjacent_view( V base );  // (2) (since C++23)
```

Constructs an `adjacent_view`.

1) Default constructor. Value-initializes the underlying view.

2) Initializes the underlying view `base_` with `std::move(base)`.

### Parameters

- **base** — the underlying view

### Example

```cpp
#include <iostream>
#include <ranges>
#include <string>
#include <tuple>

template<class... Ts>
void print(std::tuple<Ts...> const& tuple)
{
    std::apply([&](auto&& arg, auto&&... args)
    {
        std::cout << arg;
        ((std::cout << args), ...);
    }, tuple);
    std::cout << '\n';
}

int main()
{
    const std::string v{"ABCDEF"};
    constexpr int window_size{4};

    std::cout << "v: " << v << '\n';

    auto view = std::views::adjacent<window_size>(v); // overload (2)

    for (auto const& tuple : view)
        print(tuple);
}
```

Output:

```text
v: ABCDEF
ABCD
BCDE
CDEF
```

---
*Source: https://en.cppreference.com/w/cpp/ranges/adjacent_view/adjacent_view*
