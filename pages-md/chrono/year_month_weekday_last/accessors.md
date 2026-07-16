# std::chrono::year_month_weekday_last::year, std::chrono::year_month_weekday_last::month, std::chrono::year_month_weekday_last::weekday, std::chrono::year_month_weekday_last::weekday_last

```cpp
constexpr std::chrono::year year() const noexcept;  // (1) (since C++20)
constexpr std::chrono::month month() const noexcept;  // (2) (since C++20)
constexpr std::chrono::weekday weekday() const noexcept;  // (3) (since C++20)
constexpr std::chrono::weekday_last weekday_last() const noexcept;  // (4) (since C++20)
```

Retrieves the field values stored in this `year_month_weekday_last` object.

### Return value

1) Returns the stored `std::chrono::year` value.

2) Returns the stored `std::chrono::month` value.

3) Returns the stored `std::chrono::weekday` value.

4) `std::chrono::weekday_last(weekday())`

### Example

```cpp
#include <chrono>
#include <iostream>
using namespace std::chrono;

int main()
{
    auto ymwdl{Tuesday[last]/November/2022};
    auto wdl = ymwdl.weekday_last();
    wdl = (wdl.weekday() + days(1))[last];
    ymwdl = {ymwdl.year() + years(1), ymwdl.month() - months(2), wdl};
    std::cout << year_month_day(ymwdl) << '\n';
}
```

Output:

```text
2023-09-27
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/year_month_weekday_last/accessors*
