# std::dynamic_extent

```cpp
inline constexpr std::size_t dynamic_extent = std::numeric_limits<std::size_t>::max();  // (since C++20)
```

`std::dynamic_extent` is a constant of type `std::size_t` that is used to
differentiate `std::span` of static and dynamic extent.

### Note

Since `std::size_t` is an unsigned type, an equivalent definition is:

```cpp
inline constexpr std::size_t dynamic_extent = -1;
```

See integral conversions.

### Example

```cpp
#include <array>
#include <cassert>
#include <cstddef>
#include <iostream>
#include <span>
#include <string_view>
#include <vector>

int main()
{
    auto print = [](std::string_view const name, std::size_t ex)
    {
        std::cout << name << ", ";
        if (std::dynamic_extent == ex)
            std::cout << "dynamic extent\n";
        else
            std::cout << "static extent = " << ex << '\n';
    };

    int a[]{1, 2, 3, 4, 5};

    std::span span1{a};
    print("span1", span1.extent);

    std::span<int, std::dynamic_extent> span2{a};
    print("span2", span2.extent);

    std::array ar{1, 2, 3, 4, 5};
    std::span span3{ar};
    print("span3", span3.extent);

    std::vector v{1, 2, 3, 4, 5};
    std::span span4{v};
    print("span4", span4.extent);
}
```

Output:

```text
span1, static extent = 5
span2, dynamic extent
span3, static extent = 5
span4, dynamic extent
```

### See also

- **span (C++20)** — a non-owning view over a contiguous sequence of objects
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/container/span/dynamic_extent*
