# std::chrono::year_month_day_last

```cpp
class year_month_day_last;  // (since C++20)
```

The class `year_month_day_last` represents the last day of a specific year and
month. It is a field-based time point, with a resolution of `std::chrono::days`,
subject to the limit that it can only represent the last day of a month.

`std::chrono::years`- and `std::chrono::months`-oriented arithmetic are
supported directly. An implicit conversion to `std::chrono::sys_days` allows
`std::chrono::days`-oriented arithmetic to be performed efficiently.

`year_month_day_last` is a TriviallyCopyable StandardLayoutType.

### Member functions

- **(constructor)** — constructs a `year_month_day_last` object (public member
  function)
- **operator+=operator-=** — modifies the time point by some number of months or
  years (public member function)
- **yearmonthdaymonth_day_last** — accesses the fields of this object (public
  member function)
- **operator sys_daysoperator local_days** — converts to a
  `std::chrono::time_point` (public member function)
- **ok** — checks whether this object represents a valid date (public member
  function)

### Nonmember functions

- **operator==operator<=> (C++20)** — compares two `year_month_day_last` values
  (function)
- **operator+operator- (C++20)** — adds or subtracts a `year_month_day_last` and
  some number of years or months (function)
- **operator<< (C++20)** — outputs a `year_month_day_last` into a stream
  (function template)

### Helper classes

- **std::formatter<std::chrono::year_month_day_last> (C++20)** — formatting
  support for `year_month_day_last` (class template specialization)
- **std::hash<std::chrono::year_month_day_last> (C++26)** — hash support for
  **`std::chrono::year_month_day_last`** (class template specialization)

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    const auto ymd = std::chrono::year_month_day
    {
        std::chrono::floor<std::chrono::days>(std::chrono::system_clock::now())
    };

    const std::chrono::year_month_day_last ymdl
    {
        ymd.year(), ymd.month() / std::chrono::last
    };

    std::cout << "The last day of present month (" << ymdl << ") is: "
              << std::chrono::year_month_day{ymdl}.day() << '\n';

    // The 'last' object can be placed wherever it is legal to place a 'day':
    using namespace std::chrono;
    constexpr std::chrono::year_month_day_last
        ymdl1 = 2023y / February / last,
        ymdl2 = last / February / 2023y,
        ymdl3 = February / last / 2023y;
    static_assert(ymdl1 == ymdl2 && ymdl2 == ymdl3);
}
```

Possible output:

```text
The last day of present month (2023/Aug/last) is: 31
```

### See also

- **year_month_day (C++20)** — represents a specific `year`, `month`, and `day`
  (class)

---
*Source: https://en.cppreference.com/w/cpp/chrono/year_month_day_last*
