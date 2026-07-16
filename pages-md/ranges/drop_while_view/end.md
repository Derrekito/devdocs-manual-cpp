# std::ranges::drop_while_view<V,Pred>::end

```cpp
constexpr auto end();  // (since C++20)
```

Returns a sentinel or an iterator representing the end of the `drop_while_view`.

Effectively returns `ranges::end(base_)`, where `base_` is the underlying view.

### Parameters

(none)

### Return value

A sentinel or an iterator representing the end of the view.

### Example

```cpp
#include <array>
#include <iostream>
#include <ranges>

int main()
{
    constexpr std::array data{0, -1, -2, 3, 1, 4, 1, 5};

    auto view = std::ranges::drop_while_view
    {
        data, [](int x) { return x <= 0; }
    };

    for (auto it = view.begin(); it != view.end(); ++it)
        std::cout << *it << ' ';
    std::cout << '\n';
}
```

Output:

```text
3 1 4 1 5
```

### See also

- **begin (C++20)** — returns an iterator to the beginning (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/drop_while_view/end*
