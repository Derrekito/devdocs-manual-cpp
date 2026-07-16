# std::chrono::month_weekday::month, std::chrono::month_weekday::weekday_indexed

```cpp
constexpr std::chrono::month month() const noexcept;  // (1) (since C++20)
constexpr std::chrono::weekday_indexed weekday_indexed() const noexcept;  // (2) (since C++20)
```

Retrieves a copy of the `month` and `weekday_indexed` objects stored in `*this`.

### Return value

1) A copy of the `std::chrono::month` object stored in `*this`.

2) A copy of the `std::chrono::weekday_indexed` object stored in `*this`.

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    std::cout << std::boolalpha;

    auto mwdi{std::chrono::March/std::chrono::Friday[1]}; // 1st Friday in a March
    std::cout << (std::chrono::year_month_day{mwdi/2024} ==
                  std::chrono::year_month_day{std::chrono::March/1/2024})
                  << ' ';
    auto index = mwdi.weekday_indexed().index();
    auto weekday = mwdi.weekday_indexed().weekday();
    mwdi = {mwdi.month(), weekday[index + 4]}; // 5th Friday in a March
    std::cout << (std::chrono::year_month_day{mwdi/2024} ==
                  std::chrono::year_month_day{std::chrono::March/29/2024})
                  << '\n';
}
```

Output:

```text
true true
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/month_weekday/accessors*
