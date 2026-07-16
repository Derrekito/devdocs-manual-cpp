# std::ranges::chunk_view<V>::chunk_view

```cpp
constexpr explicit chunk_view( V base, ranges::range_difference_t<V> n );  // (since C++23)
```

Constructs a `chunk_view`, initializing the underlying data members:

- move constructs the `base_` with `std::move(base)`,
- initializes the "chunk size" `n_` with `n`.

In addition, if `V` models exactly the `input_range`, the constructor
initializes the following exposition-only data members:

- initializes the `remainder_` with `​0​`,
- default-initializes the `current_`.

The behavior is undefined if `n` is less than or equal to `​0​`.

### Parameters

- **base** — the adapted view
- **n** — the chunk size

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <ranges>

int main()
{
    auto i = std::views::iota(0, 10);
    auto w = std::ranges::chunk_view(i, 4);

    std::ranges::for_each(w, [](auto const v)
    {
        for (auto e : v)
            std::cout << e << ' ';
        std::cout << '\n';
    });
}
```

Output:

```text
0 1 2 3
4 5 6 7
8 9
```

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_view/chunk_view*
