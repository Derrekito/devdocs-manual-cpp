# std::timespec_get

```cpp
int timespec_get( std::timespec* ts, int base );  // (1) (since C++17)
#define TIME_UTC /* implementation-defined */  // (2) (since C++17)
```

1) Modifies the `std::timespec` object pointed to by `ts` to hold the current
   calendar time in the time base `base`.

2) Expands to a value suitable for use as the `base` argument of
   `std::timespec_get`.

Other macro constants beginning with `TIME_` may be provided by the
implementation to indicate additional time bases.

If `base` is `TIME_UTC`, then

- `ts->tv_sec` is set to the number of seconds since an implementation defined
  epoch, truncated to a whole value,
- `ts->tv_nsec` member is set to the integral number of nanoseconds, rounded to
  the resolution of the system clock.

### Parameters

- **ts** — pointer to an object of type `std::timespec`
- **base** — `TIME_UTC` or another nonzero integer value indicating the time
  base

### Return value

The value of `base` if successful, zero otherwise.

### Notes

The POSIX function `clock_gettime(CLOCK_REALTIME, ts)` may also be used to
populate a `std::timespec` with the time since the Epoch.

### Example

```cpp
#include <ctime>
#include <iostream>

int main()
{
    std::timespec ts;
    std::timespec_get(&ts, TIME_UTC);
    char buf[100];
    std::strftime(buf, sizeof buf, "%D %T", std::gmtime(&ts.tv_sec));
    std::cout << "Current time: " << buf << '.' << ts.tv_nsec << " UTC\n";
}
```

Possible output:

```text
Current time: 06/24/16 20:07:42.949494132 UTC
```

### See also

- **timespec (C++17)** — time in seconds and nanoseconds (struct)
- **time** — returns the current time of the system as time since epoch
  (function)

**C documentation for `timespec_get`**

---
*Source: https://en.cppreference.com/w/cpp/chrono/c/timespec_get*
