# std::gmtime

```cpp
std::tm* gmtime( const std::time_t* time );
```

Converts given time since epoch as `std::time_t` value into calendar time,
expressed in Coordinated Universal Time (UTC).

### Parameters

- **time** — pointer to a time_t object to convert

### Return value

Pointer to a static internal `std::tm` object on success, or null pointer
otherwise. The structure may be shared between `std::gmtime`, `std::localtime`,
and `std::ctime` and may be overwritten on each invocation.

### Notes

This function may not be thread-safe.

POSIX requires that this function sets `errno` to `EOVERFLOW` if it fails
because the argument is too large.

### Example

```cpp
#include <ctime>
#include <iomanip>
#include <iostream>
#include <sstream>

int main()
{
    setenv("TZ", "/usr/share/zoneinfo/Europe/London", 1); // POSIX-specific

    std::tm tm{}; // get_time does not set all fields hence {}
    tm.tm_year = 2020 - 1900; // 2020
    tm.tm_mon = 7 - 1; // July
    tm.tm_mday = 15; // 15th
    tm.tm_hour = 10;
    tm.tm_min = 15;
    tm.tm_isdst = 1; // Daylight saving in London
    std::time_t t = std::mktime(&tm);

    std::cout << "UTC:   " << std::put_time(std::gmtime(&t), "%c %Z") << '\n';
    std::cout << "local: " << std::put_time(std::localtime(&t), "%c %Z") << '\n';
}
```

Possible output:

```text
UTC:   Wed Jul 15 09:15:00 2020 GMT
local: Wed Jul 15 10:15:00 2020 BST
```

### See also

- **localtime** — converts time since epoch to calendar time expressed as local
  time (function)

**C documentation for `gmtime`**

---
*Source: https://en.cppreference.com/w/cpp/chrono/c/gmtime*
