# std::ranges::lazy_split_view<V,Pattern>::base

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
#include <string_view>

void print(std::string_view rem, auto const& r, std::string_view post = "\n") {
    for (std::cout << rem; auto const& e : r)
        std::cout << e;
    std::cout << post;
}

int main()
{
    constexpr std::string_view keywords{ "this,..throw,..true,..try,.." };
    constexpr std::string_view pattern{",.."};
    constexpr std::ranges::lazy_split_view lazy_split_view{keywords, pattern};
    print("base() = [", lazy_split_view.base(), "]\n"
          "substrings: ");
    for (auto const& split: lazy_split_view)
        print("[", split, "] ");
}
```

Output:

```text
base() = [this,..throw,..true,..try,..]
substrings: [this] [throw] [true] [try] []
```

### See also

- **base (C++20)** — returns a copy of the underlying (adapted) view (public
  member function of `std::ranges::split_view<V,Pattern>`)

---
*Source: https://en.cppreference.com/w/cpp/ranges/lazy_split_view/base*
