# std::chrono::year_month_day::year, std::chrono::year_month_day::month, std::chrono::year_month_day::day

```cpp
constexpr std::chrono::year year() const noexcept;  // (1) (since C++20)
constexpr std::chrono::month month() const noexcept;  // (2) (since C++20)
constexpr std::chrono::day day() const noexcept;  // (3) (since C++20)
```

Retrieves the year, month and day values stored in this `year_month_day` object.

### Return value

1) Returns the stored `std::chrono::year` value.

2) Returns the stored `std::chrono::month` value.

3) Returns the stored `std::chrono::day` value.

### Example

```cpp
#include <chrono>

int main()
{
    constexpr std::chrono::year_month_day ymd{std::chrono::July/1/2021};

    static_assert(ymd.year() == std::chrono::year(2021));
    static_assert(ymd.month() == std::chrono::month(7));
    static_assert(ymd.day() == std::chrono::day(1));
}
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/year_month_day/accessors*
