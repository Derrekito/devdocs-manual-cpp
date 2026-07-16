# std::span<T,Extent>::rend, std::span<T,Extent>::crend

```cpp
constexpr reverse_iterator rend() const noexcept;  // (1) (since C++20)
constexpr const_reverse_iterator crend() const noexcept;  // (2) (since C++23)
```

Returns a reverse iterator to the element following the last element of the
reversed `span`. It corresponds to the element preceding the first element of
the non-reversed `span`. This element acts as a placeholder, attempting to
access it results in undefined behavior.

### Parameters

(none)

### Return value

Reverse iterator to the element following the last element.

### Complexity

Constant.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <span>
#include <string_view>

void ascending(const std::span<const std::string_view> data,
               const std::string_view term)
{
    std::for_each(data.begin(), data.end(),
        [](const std::string_view x) { std::cout << x << ' '; });
    std::cout << term;
}

void descending(const std::span<const std::string_view> data,
                const std::string_view term)
{
    std::for_each(data.rbegin(), data.rend(),
        [](const std::string_view x) { std::cout << x << ' '; });
    std::cout << term;
}

int main()
{
    constexpr std::string_view bars[]{"▁","▂","▃","▄","▅","▆","▇","█"};
    ascending(bars, " ");
    descending(bars, "\n");
}
```

Output:

```text
▁ ▂ ▃ ▄ ▅ ▆ ▇ █  █ ▇ ▆ ▅ ▄ ▃ ▂ ▁
```

### See also

- **rbegincrbegin (C++23)** — returns a reverse iterator to the beginning
  (public member function)
- **rendcrend (C++14)** — returns a reverse end iterator for a container or
  array (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/span/rend*
