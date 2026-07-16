# std::tm

```cpp
struct tm;
```

Structure holding a calendar date and time broken down into its components.

### Member objects

- **int tm_sec** — seconds after the minute – `[``​0​``,``61``]`(until C++11)
  `[``​0​``,``60``]`(since C++11)[note 1] (public member object)
- **int tm_min** — minutes after the hour – `[``​0​``,``59``]` (public member
  object)
- **int tm_hour** — hours since midnight – `[``​0​``,``23``]` (public member
  object)
- **int tm_mday** — day of the month – `[``1``,``31``]` (public member object)
- **int tm_mon** — months since January – `[``​0​``,``11``]` (public member
  object)
- **int tm_year** — years since 1900 (public member object)
- **int tm_wday** — days since Sunday – `[``​0​``,``6``]` (public member object)
- **int tm_yday** — days since January 1 – `[``​0​``,``365``]` (public member
  object)
- **int tm_isdst** — Daylight Saving Time flag. The value is positive if DST is
  in effect, zero if not and negative if no information is available. (public
  member object)

Notes

The Standard mandates only the presence of the aforementioned members in some
order. The implementations usually add more data members to this structure.

1. Range allows for a positive leap second. Two leap seconds in the same minute
   are not allowed (the range `[``​0​``,``61``]` was a defect introduced in C89
   and corrected in C99).

### Example

```cpp
#include <ctime>
#include <iostream>

int main()
{
    std::tm tm{};
    tm.tm_year = 2022 - 1900;
    tm.tm_mday = 1;
    std::mktime(&tm);

    std::cout << std::asctime(&tm); // note implicit trailing '\n'

    std::cout << "sizeof(std::tm) = " << sizeof(std::tm) << '\n'
              << "sum of sizes of standard members = "
              << sizeof(tm.tm_sec) +
                 sizeof(tm.tm_min) +
                 sizeof(tm.tm_hour) +
                 sizeof(tm.tm_mday) +
                 sizeof(tm.tm_mon) +
                 sizeof(tm.tm_year) +
                 sizeof(tm.tm_wday) +
                 sizeof(tm.tm_yday) +
                 sizeof(tm.tm_isdst) << '\n';
}
```

Possible output:

```text
Sat Jan  1 00:00:00 2022
sizeof(std::tm) = 56
sum of sizes of standard members = 36
```

### See also

- **localtime** — converts time since epoch to calendar time expressed as local
  time (function)
- **gmtime** — converts time since epoch to calendar time expressed as Universal
  Coordinated Time (function)

**C documentation for `tm`**

---
*Source: https://en.cppreference.com/w/cpp/chrono/c/tm*
