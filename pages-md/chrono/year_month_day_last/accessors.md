# std::chrono::year_month_day_last::year, std::chrono::year_month_day_last::month, std::chrono::year_month_day_last::day, std::chrono::year_month_day_last::month_day_last

```cpp
constexpr std::chrono::year year() const noexcept;  // (1) (since C++20)
constexpr std::chrono::month month() const noexcept;  // (2) (since C++20)
constexpr std::chrono::day day() const noexcept;  // (3) (since C++20)
constexpr std::chrono::month_day_last month_day_last() const noexcept;  // (4) (since C++20)
```

Retrieves the field values stored in this `year_month_day_last` object.

### Return value

1) Returns the stored `std::chrono::year` value.

2) Returns the stored `std::chrono::month` value.

3) A `std::chrono::day` value corresponding to the last day of the stored year
   and month. If `this->ok()` is `false`, the return value is unspecified.

4) A `std::chrono::month_day_last` constructed from `month()`.

### Example

```cpp
#include <chrono>

int main()
{
    constexpr auto ymdl{std::chrono::last/11/2020};
    static_assert
    (
        ymdl.day() == std::chrono::day(30) &&
        ymdl.month() == std::chrono::November &&
        ymdl.year() == std::chrono::year(2020) &&
        ymdl.month_day_last() == std::chrono::November/std::chrono::last
    );
}
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/year_month_day_last/accessors*
