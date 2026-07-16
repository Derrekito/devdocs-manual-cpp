# std::chrono::year_month_day::year_month_day

```cpp
year_month_day() = default;  // (1) (since C++20)
constexpr year_month_day( const std::chrono::year& y,
                          const std::chrono::month& m,
                          const std::chrono::day& d ) noexcept;  // (2) (since C++20)
constexpr year_month_day( const std::chrono::year_month_day_last& ymdl ) noexcept;  // (3) (since C++20)
constexpr year_month_day( const std::chrono::sys_days& dp ) noexcept;  // (4) (since C++20)
constexpr explicit year_month_day( const std::chrono::local_days& dp ) noexcept;  // (5) (since C++20)
```

Constructs a `year_month_day` object.

1) Default constructor leaves the date uninitialized.

2) Constructs a `year_month_day` object that stores the year `y`, month `m` and
   day `d`.

3) Constructs a `year_month_day` object that stores the year `ymdl.year()`,
   month `ymdl.month()` and day `ymdl.day()`.

4) Constructs a `year_month_day` object that represent the same date as the one
   represented by `dp`.

5) Constructs a `year_month_day` object that represent the same date as the one
   represented by `dp`, as if by
   `year_month_day(sys_days(dp.time_since_epoch()))`.

Constructors (3,4) define implicit conversions from
`std::chrono::year_month_day_last` and `std::chrono::sys_days`, respectively.

For any `year_month_day` object `ymd` representing a valid date (`ymd.ok() ==
true`), converting `ymd` to `sys_days` and back yields the same value.

### Notes

A `year_month_day` can also be created by combining one of the partial-date
types `std::chrono::year_month` and `std::chrono::month_day` with the missing
component (day and year, respectively) using `operator/`.

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    using namespace std::chrono;

    constexpr auto ymd2 = std::chrono::year_month_day(
        2020y, std::chrono::January, 31d // overload (2)
    );
    std::cout << "ymd2: " << ymd2 << '\n';

    constexpr auto ymd3 = std::chrono::year_month_day(
        std::chrono::April / std::chrono::Monday[last] / 2023y // overload (3)
    );
    std::cout << "ymd3: " << ymd3 << '\n';

    const auto now = std::chrono::system_clock::now();
    const auto ymd4 = std::chrono::year_month_day(
        std::chrono::floor<std::chrono::days>(now) // overload (4)
    );
    std::cout << "ymd4: " << ymd4 << '\n';
}
```

Possible output:

```text
ymd2: 2020-01-31
ymd3: 2023-04-24
ymd4: 2023-08-30
```

### See also

- **operator/ (C++20)** — conventional syntax for Gregorian calendar date
  creation (function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/year_month_day/year_month_day*
