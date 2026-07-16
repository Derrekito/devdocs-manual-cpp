# std::chrono::system_clock

```cpp
class system_clock;  // (since C++11)
```

Class `std::chrono::system_clock` represents the system-wide real time wall
clock.

It may not be monotonic: on most systems, the system time can be adjusted at any
moment. It is the only C++ clock that has the ability to map its time points to
C-style time.

`std::chrono::system_clock` meets the requirements of TrivialClock.

The epoch of `system_clock` is unspecified, but most implementations use Unix
Time (i.e., time since 00:00:00 Coordinated Universal Time (UTC), Thursday, 1
January 1970, not counting leap seconds).
*(until C++20)*

`system_clock` measures Unix Time (i.e., time since 00:00:00 Coordinated
Universal Time (UTC), Thursday, 1 January 1970, not counting leap seconds).
*(since C++20)*

### Time point family

```cpp
Defined in namespace std::chrono
template<class Duration>
using sys_time = std::chrono::time_point<std::chrono::system_clock, Duration>;  // (since C++20)
using sys_seconds = sys_time<std::chrono::seconds>;  // (since C++20)
using sys_days = sys_time<std::chrono::days>;  // (since C++20)
```

- **operator<<(std::chrono::sys_time) (C++20)** — performs stream output on a
  `sys_time` (function template)
- **from_stream(std::chrono::sys_time) (C++20)** — parses a `sys_time` from a
  stream according to the provided format (function template)
- **std::formatter<std::chrono::sys_time> (C++20)** — formatting support for
  `sys_time` (class template specialization)

### Member types

- **`rep`** — signed arithmetic type representing the number of ticks in the
  clock's duration
- **`period`** — a `std::ratio` type representing the tick period of the clock,
  in seconds
- **`duration`** — `std::chrono::duration<rep, period>`, capable of representing
  negative durations
- **`time_point`** — `std::chrono::time_point<std::chrono::system_clock>`

### Member constants

- **constexpr bool is_steady [static]** — `true` if the time between ticks is
  always constant, i.e. calls to `now()` return values that increase
  monotonically even in case of some external clock adjustment, otherwise
  `false` (public static member constant)

### Member functions

- **now [static]** — returns a `std::chrono::time_point` representing the
  current point in time (public static member function)
- **to_time_t [static]** — converts a system clock time point to `std::time_t`
  (public static member function)
- **from_time_t [static]** — converts `std::time_t` to a system clock time point
  (public static member function)

### Notes

The `system_clock`'s time value can be internally adjusted at any time by the
operating system, for example due to NTP synchronization or the user changing
the system's clock. Daylight Saving Time and time zone changes, however, do not
affect it since it is based on the UTC time-zone.

### See also

- **steady_clock (C++11)** — monotonic clock that will never be adjusted (class)
- **high_resolution_clock (C++11)** — the clock with the shortest tick period
  available (class)

---
*Source: https://en.cppreference.com/w/cpp/chrono/system_clock*
