# std::chrono::month_weekday_last::month, std::chrono::month_weekday_last::weekday_last

```cpp
constexpr std::chrono::month month() const noexcept;  // (1) (since C++20)
constexpr std::chrono::weekday_last weekday_last() const noexcept;  // (2) (since C++20)
```

Retrieves a copy of the `month` and `weekday_last` objects stored in `*this`.

### Return value

1) A copy of the `std::chrono::month` object stored in `*this`.

2) A copy of the `std::chrono::weekday_last` object stored in `*this`.

### Example

```cpp
#include <chrono>
#include <iostream>
using namespace std::chrono;

int main()
{
    std::cout << std::boolalpha;

    auto mwdl{March/Friday[last]}; // Last Friday in a March
    auto ywdl{year(2024)/mwdl};
    std::cout << (year_month_day{ywdl} ==
                  year_month_day{March/29/2024}) << ' ';
    // Last Friday of the next month, in 2024
    mwdl = {(mwdl.month() + months(1))/mwdl.weekday_last()};
    ywdl = {year(2024)/mwdl};
    std::cout << (year_month_day{ywdl} ==
                  year_month_day{April/26/2024}) << '\n';
}
```

Output:

```text
true true
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/month_weekday_last/accessors*
