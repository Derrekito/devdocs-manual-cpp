# std::full_extent, std::full_extent_t

```cpp
struct full_extent_t { explicit full_extent_t() = default; };  // (1) (since C++26)
inline constexpr std::full_extent_t full_extent {};  // (2) (since C++26)
```

1) The class `std::full_extent_t` is a slice specifier type that can be used in
   `std::submdspan`.

2) The corresponding `std::full_extent` instance of (1) is a slice specifier to
   indicate full range of indices in the specified extent in `std::submdspan`.

### Example

```cpp
#include <mdspan>
#include <print>

void print_view(auto view)
{
    static_assert(view.rank() <= 2);

    if constexpr (view.rank() == 2)
    {
      for (std::size_t i = 0; i < view.extent(0); ++i)
      {
        for (std::size_t j = 0; j < view.extent(1); ++j)
          std::print("{} ", view[i, j]);
        std::println("");
      }
    }
    else if constexpr (view.rank() == 1)
    {
      for (std::size_t i = 0; i < view.extent(0); ++i)
        std::print("{} ", view[i]);
      std::println("");
    }
    else
    {
      std::println("{}", view[]);
    }
    std::println("");
}

int main()
{
    char letters [] { 'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I' };
    std::mdspan view (letters, 3, 3);

    print_view(view);
    print_view(std::submdspan(view, std::full_extent, std::full_extent));
    print_view(std::submdspan(view, std::full_extent, 1));
    print_view(std::submdspan(view, 1, std::full_extent));
    print_view(std::submdspan(view, 2, 1));
}
```

Possible output:

```text
A B C
D E F
G H I

A B C
D E F
G H I

B E H

D E F

H
```

### See also

- **submdspan (C++26)** — returns a view of a subset of an existing `mdspan`
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/mdspan/full_extent*
