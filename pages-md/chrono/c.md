# C Date and time utilities

### Functions

**Time manipulation**

- **difftime** — computes the difference between times (function)
- **time** — returns the current time of the system as time since epoch
  (function)
- **clock** — returns raw processor clock time since the program is started
  (function)
- **timespec_get (C++17)** — returns the calendar time in seconds and
  nanoseconds based on a given time base (function)

**Format conversions**

- **asctime** — converts a `std::tm` object to a textual representation
  (function)
- **ctime** — converts a `std::time_t` object to a textual representation
  (function)
- **strftime** — converts a `std::tm` object to custom textual representation
  (function)
- **wcsftime** — converts a `std::tm` object to custom wide string textual
  representation (function)
- **gmtime** — converts time since epoch to calendar time expressed as Universal
  Coordinated Time (function)
- **localtime** — converts time since epoch to calendar time expressed as local
  time (function)
- **mktime** — converts calendar time to time since epoch (function)

### Constants

- **CLOCKS_PER_SEC** — number of processor clock ticks per second (macro
  constant)

### Types

- **tm** — calendar time type (class)
- **time_t** — time since epoch type (typedef)
- **clock_t** — process running time (typedef)
- **timespec (C++17)** — time in seconds and nanoseconds (struct)

### See also

**C documentation for Date and time utilities**

---
*Source: https://en.cppreference.com/w/cpp/chrono/c*
