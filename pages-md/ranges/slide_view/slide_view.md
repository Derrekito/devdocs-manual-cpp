# std::ranges::slide_view<V>::slide_view

```cpp
constexpr explicit slide_view( V base, ranges::range_difference_t<V> n );  // (since C++23)
```

Constructs a `slide_view` initializing the underlying data members:

- move construct the underlying view `base_` with `std::move(base)`,
- the "window size" `n_` with `n`.

### Parameters

- **base** — the source view
- **n** — the "sliding window" size

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <ranges>

int main()
{
    const auto source = {1, 2, 3, 4};

    auto slide = std::views::slide(source, 3);

    std::ranges::for_each(slide, [](std::ranges::viewable_range auto&& w)
    {
        std::cout << '[' << w[0] << ' ' << w[1] << ' ' << w[2] << "]\n";
    });
}
```

Output:

```text
[1 2 3]
[2 3 4]
```

---
*Source: https://en.cppreference.com/w/cpp/ranges/slide_view/slide_view*
