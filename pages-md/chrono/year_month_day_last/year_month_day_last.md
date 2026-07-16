# std::chrono::year_month_day_last::year_month_day_last

```cpp
constexpr year_month_day_last( const std::chrono::year& y,
                               const std::chrono::month_day_last& mdl ) noexcept;  // (since C++20)
```

Constructs a `year_month_day_last` object storing the year `y` and the month
`mdl.month()`. The constructed object represents the last day of that year and
month.

### Notes

A `year_month_day_last` can also be created by combining a
`std::chrono::year_month` with `std::chrono::last` using `operator/`.

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    std::cout << "All last days of months of a given year:\n";

    constexpr std::chrono::year year{2021};

    for (int i{1}; i <= 12; ++i)
    {
        const std::chrono::month_day_last mdl{std::chrono::month(i) / std::chrono::last};
        const std::chrono::year_month_day_last ymdl{year, mdl};
        std::cout << ymdl << " day: " << ymdl.day() << '\n';
    }
}
```

Possible output:

```text
All last days of months of a given year:
2021/Jan/last day: 31
2021/Feb/last day: 28
2021/Mar/last day: 31
2021/Apr/last day: 30
2021/May/last day: 31
2021/Jun/last day: 30
2021/Jul/last day: 31
2021/Aug/last day: 31
2021/Sep/last day: 30
2021/Oct/last day: 31
2021/Nov/last day: 30
2021/Dec/last day: 31
```

### See also

- **operator/ (C++20)** — conventional syntax for Gregorian calendar date
  creation (function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/year_month_day_last/year_month_day_last*
