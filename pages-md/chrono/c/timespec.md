# std::timespec

```cpp
struct timespec;  // (since C++17)
```

Structure holding an interval broken down into seconds and nanoseconds.

### Member objects

- **`std::time_t` `tv_sec`** — whole seconds – >= 0
- **`long` `tv_nsec`** — nanoseconds – `[``​0​``,``999999999``]`

The declaration order of `tv_sec` and `tv_nsec` is unspecified. Implementation
may add other data members to `timespec`.

### Notes

The type of `tv_nsec` is `long long` on some platforms, which is currently
non-conforming in C++, but is conforming since C23.

### Example

```cpp
#include <ctime>
#include <iostream>

int main()
{
    std::timespec ts;
    std::timespec_get(&ts, TIME_UTC);
    char buff[0x80];
    std::strftime(buff, sizeof buff, "%D %T", std::gmtime(&ts.tv_sec));

//  auto [sec, nsec] = ts; // UB: structured bindings should not be used because the
                           // declaration order and data member list are unspecified

    std::cout << "Current time: " << buff << " (UTC)\n"
              << "Raw timespec.time_t: " << ts.tv_sec << '\n'
              << "Raw timespec.tv_nsec: " << ts.tv_nsec << '\n';
}
```

Possible output:

```text
Current time: 04/06/23 12:03:31 (UTC)
Raw timespec.time_t: 1680782611
Raw timespec.tv_nsec: 678437213
```

### See also

- **timespec_get (C++17)** — returns the calendar time in seconds and
  nanoseconds based on a given time base (function)
- **tm** — calendar time type (class)

**C documentation for `timespec`**

---
*Source: https://en.cppreference.com/w/cpp/chrono/c/timespec*
