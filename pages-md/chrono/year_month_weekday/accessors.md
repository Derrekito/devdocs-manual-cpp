# std::chrono::year_month_weekday::year, std::chrono::year_month_weekday::month, std::chrono::year_month_weekday::weekday, std::chrono::year_month_weekday::index, std::chrono::year_month_weekday::weekday_indexed

```cpp
constexpr std::chrono::year year() const noexcept;  // (1) (since C++20)
constexpr std::chrono::month month() const noexcept;  // (2) (since C++20)
constexpr std::chrono::weekday weekday() const noexcept;  // (3) (since C++20)
constexpr unsigned index() const noexcept;  // (4) (since C++20)
constexpr std::chrono::weekday_indexed weekday_indexed() const noexcept;  // (5) (since C++20)
```

Retrieves the field values stored in this `year_month_weekday` object.

### Return value

1) Returns the stored `std::chrono::year` value.

2) Returns the stored `std::chrono::month` value.

3) Returns the stored `std::chrono::weekday` value.

4) Returns the stored weekday index.

5) `weekday()[index()]`

### Example

```cpp
#include <cassert>
#include <chrono>

int main()
{
    constexpr auto ym{std::chrono::year(2021)/std::chrono::January};
    constexpr auto wdi{std::chrono::Wednesday[1]};
    auto ymwdi{ym/wdi};
    const auto index{ymwdi.index() + 1};
    auto weekday{ymwdi.weekday() + std::chrono::days(1)};
    ymwdi = {ymwdi.year()/ymwdi.month()/weekday[index]};
    // Second Thursday in January, 2021
    assert(std::chrono::year_month_day{ymwdi} == std::chrono::January/14/2021);
}
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/year_month_weekday/accessors*
