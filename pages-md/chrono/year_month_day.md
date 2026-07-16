# std::chrono::year_month_day

```cpp
class year_month_day;  // (since C++20)
```

The class `year_month_day` represents a specific year, month, and day. It is a
field-based time point, with a resolution of `std::chrono::days`.
`std::chrono::years`- and `std::chrono::months`-oriented arithmetic are
supported directly. An implicit conversion to and from `std::chrono::sys_days`
allows `std::chrono::days`-oriented arithmetic to be performed efficiently.

`year_month_day` is a TriviallyCopyable StandardLayoutType.

### Member functions

- **(constructor)** — constructs a `year_month_day` (public member function)
- **operator+=operator-=** — modifies the time point by some number of months or
  years (public member function)
- **yearmonthday** — accesses the year, month, and day stored in this object
  (public member function)
- **operator sys_daysoperator local_days** — converts to a
  `std::chrono::time_point` (public member function)
- **ok** — checks if the `year_month_day` represents a valid date (public member
  function)

### Nonmember functions

- **operator==operator<=> (C++20)** — compares two `year_month_day` values
  (function)
- **operator+operator- (C++20)** — adds or subtracts a `year_month_day` and some
  number of years or months (function)
- **operator<< (C++20)** — outputs a `year_month_day` into a stream (function
  template)
- **from_stream (C++20)** — parses a `year_month_day` from a stream according to
  the provided format (function template)

### Helper classes

- **std::formatter<std::chrono::year_month_day> (C++20)** — formatting support
  for `year_month_day` (class template specialization)
- **std::hash<std::chrono::year_month_day> (C++26)** — hash support for
  **`std::chrono::year_month_day`** (class template specialization)

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    const std::chrono::time_point now{std::chrono::system_clock::now()};

    const std::chrono::year_month_day ymd{std::chrono::floor<std::chrono::days>(now)};

    std::cout << "Current Year: " << static_cast<int>(ymd.year()) << ", "
                 "Month: " << static_cast<unsigned>(ymd.month()) << ", "
                 "Day: " << static_cast<unsigned>(ymd.day()) << "\n"
                 "ymd: " << ymd << '\n';
}
```

Possible output:

```text
Current Year: 2023, Month: 9, Day: 4
ymd: 2023-09-04
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/year_month_day*
