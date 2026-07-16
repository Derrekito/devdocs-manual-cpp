# std::chrono::gps_clock

```cpp
class gps_clock;  // (since C++20)
```

The clock `std::chrono::gps_clock` is a Clock that represents Global Positioning
System (GPS) time. It measures time since 00:00:00, 6 January 1980 UTC.

Leap seconds are not inserted into GPS. Thus, every time a leap second is
inserted into UTC, UTC falls another second behind GPS. As of December 2017, UTC
is 18 seconds behind GPS, reflecting the 18 leap seconds inserted between 1980
and 2017. Thus, 2018-01-01 00:00:00 UTC is equivalent to 2018-01-01 00:00:18
GPS. GPS is a constant 19 seconds behind TAI.

`gps_clock` meets the Clock requirements. It does not meet the TrivialClock
requirements unless the implementation can guarantee that `now()` does not throw
an exception.

### Time point family

```cpp
Defined in namespace std::chrono
template<class Duration>
using gps_time = std::chrono::time_point<std::chrono::gps_clock, Duration>;  // (since C++20)
using gps_seconds = gps_time<std::chrono::seconds>;  // (since C++20)
```

- **operator<<(std::chrono::gps_time) (C++20)** — performs stream output on a
  `gps_time` (function template)
- **from_stream(std::chrono::gps_time) (C++20)** — parses a `gps_time` from a
  stream according to the provided format (function template)
- **std::formatter<std::chrono::gps_time> (C++20)** — formatting support for
  `gps_time` (class template specialization)

### Member types

- **`rep`** — signed arithmetic type representing the number of ticks in the
  clock's duration
- **`period`** — a `std::ratio` type representing the tick period of the clock,
  in seconds
- **`duration`** — `std::chrono::duration<rep, period>`, capable of representing
  negative durations
- **`time_point`** — `std::chrono::time_point<std::chrono::gps_clock>`

### Member constants

- **constexpr bool is_steady [static]** — `true` if the time between ticks is
  always constant, i.e. calls to `now()` return values that increase
  monotonically even in case of some external clock adjustment, otherwise
  `false` (public static member constant)

### Member functions

- **now [static]** — returns a `std::chrono::time_point` representing the
  current point in time (public static member function)
- **to_utc [static]** — converts a `gps_time` to `utc_time` (public static
  member function)
- **from_utc [static]** — converts a `utc_time` to `gps_time` (public static
  member function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/gps_clock*
