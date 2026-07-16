# std::chrono::year_month::year, std::chrono::year_month::month

```cpp
constexpr std::chrono::year year() const noexcept;  // (1) (since C++20)
constexpr std::chrono::month month() const noexcept;  // (2) (since C++20)
```

Retrieves the year and month values stored in this `year_month` object.

### Return value

1) Returns the stored `std::chrono::year` value.

2) Returns the stored `std::chrono::month` value.

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    std::cout << std::boolalpha;

    constexpr auto ym{std::chrono::year(2021)/std::chrono::July};
    std::cout << (ym.year() == std::chrono::year(2021)) << ' ';
    std::cout << (ym.month() == std::chrono::month(7)) << '\n';
}
```

Output:

```text
true true
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/year_month/accessors*
