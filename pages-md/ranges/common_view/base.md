# std::ranges::common_view<V>::base

```cpp
constexpr V base() const& requires std::copy_constructible<V>;  // (1) (since C++20)
constexpr V base() &&;  // (2) (since C++20)
```

Returns a copy of the underlying view.

1) Copy constructs the result from the underlying view.

2) Move constructs the result from the underlying view.

### Parameters

(none)

### Return value

A copy of the underlying view.

### Example

```cpp
#include <iostream>
#include <ranges>
#include <string>

int main()
{
    std::string str { "C++20" };
    auto view = std::views::common(str);

    std::string copy_of_str = view.base();
    std::cout << "copy of str: [" << copy_of_str << "]\n";
    std::cout << "view.base(): [" << view.base() << "]\n";

    std::string move_str = std::move(view.base());
    std::cout << "moved str:   [" << move_str << "]\n";
    std::cout << "view.base(): [" << view.base() << "]\n"; // unspecified
}
```

Possible output:

```text
copy of str: [C++20]
view.base(): [C++20]
moved str:   [C++20]
view.base(): []
```

---
*Source: https://en.cppreference.com/w/cpp/ranges/common_view/base*
