# std::localtime

```cpp
std::tm* localtime( const std::time_t* time );
```

Converts given time since epoch as `std::time_t` value into calendar time,
expressed in local time.

### Parameters

- **time** — pointer to a `std::time_t` object to convert

### Return value

Pointer to a static internal `std::tm` object on success, or null pointer
otherwise. The structure may be shared between `std::gmtime`, `std::localtime`,
and `std::ctime`, and may be overwritten on each invocation.

### Notes

This function may not be thread-safe.

POSIX requires that this function sets `errno` to `EOVERFLOW` if it fails
because the argument is too large.

POSIX specifies that the timezone information is determined by this function as
if by calling `tzset`, which reads the environment variable `TZ`.

### Example

```cpp
#include <ctime>
#include <iomanip>
#include <iostream>
#include <sstream>

int main()
{
    setenv("TZ", "/usr/share/zoneinfo/America/Los_Angeles", 1); // POSIX-specific

    std::tm tm{}; // Zero initialise
    tm.tm_year = 2020 - 1900; // 2020
    tm.tm_mon = 2 - 1; // February
    tm.tm_mday = 15; // 15th
    tm.tm_hour = 10;
    tm.tm_min = 15;
    tm.tm_isdst = 0; // Not daylight saving
    std::time_t t = std::mktime(&tm);

    std::cout << "UTC:   " << std::put_time(std::gmtime(&t), "%c %Z") << '\n';
    std::cout << "local: " << std::put_time(std::localtime(&t), "%c %Z") << '\n';
}
```

Possible output:

```text
UTC:   Sat Feb 15 18:15:00 2020 GMT
local: Sat Feb 15 10:15:00 2020 PST
```

### See also

- **gmtime** — converts time since epoch to calendar time expressed as Universal
  Coordinated Time (function)
- **localtimelocaltime_rlocaltime_s (C23)(C11)** — converts time since epoch to
  calendar time expressed as local time (function)

**C documentation for `localtime`**

---
*Source: https://en.cppreference.com/w/cpp/chrono/c/localtime*
