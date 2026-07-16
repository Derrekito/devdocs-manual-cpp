# Standard library header <ctime>

This header was originally in the C standard library as `<time.h>`.

This header is part of the C-style date and time library.

**Macro constants**

- **CLOCKS_PER_SEC** — number of processor clock ticks per second (macro
  constant)
- **NULL** — implementation-defined null pointer constant (macro constant)

**Types**

- **clock_t** — process running time (typedef)
- **size_t** — unsigned integer type returned by the `sizeof` operator (typedef)
- **time_t** — time since epoch type (typedef)
- **tm** — calendar time type (class)
- **timespec (C++17)** — time in seconds and nanoseconds (struct)

**Functions**

**Time manipulation**

- **clock** — returns raw processor clock time since the program is started
  (function)
- **time** — returns the current time of the system as time since epoch
  (function)
- **difftime** — computes the difference between times (function)
- **timespec_get (C++17)** — returns the calendar time in seconds and
  nanoseconds based on a given time base (function)

**Format conversions**

- **ctime** — converts a `std::time_t` object to a textual representation
  (function)
- **asctime** — converts a `std::tm` object to a textual representation
  (function)
- **strftime** — converts a `std::tm` object to custom textual representation
  (function)
- **gmtime** — converts time since epoch to calendar time expressed as Universal
  Coordinated Time (function)
- **localtime** — converts time since epoch to calendar time expressed as local
  time (function)
- **mktime** — converts calendar time to time since epoch (function)

### Synopsis

```cpp
#define NULL /* see description */
#define CLOCKS_PER_SEC /* see description */
#define TIME_UTC /* see description */

namespace std {
  using size_t = /* see description */;
  using clock_t = /* see description */;
  using time_t = /* see description */;

  struct timespec;
  struct tm;

  clock_t clock();
  double difftime(time_t time1, time_t time0);
  time_t mktime(tm* timeptr);
  time_t time(time_t* timer);
  int timespec_get(timespec* ts, int base);
  char* asctime(const tm* timeptr);
  char* ctime(const time_t* timer);
  tm* gmtime(const time_t* timer);
  tm* localtime(const time_t* timer);
  size_t strftime(char* s, size_t maxsize, const char* format, const tm* timeptr);
}
```

### Class `std::timespec`

```cpp
struct timespec {
  std::time_t tv_sec;
  long tv_nsec;
};
```

### Class `std::tm`

```cpp
struct tm {
  int tm_sec;
  int tm_min;
  int tm_hour;
  int tm_mday;
  int tm_mon;
  int tm_year;
  int tm_wday;
  int tm_yday;
  int tm_isdst;
};
```

---
*Source: https://en.cppreference.com/w/cpp/header/ctime*
