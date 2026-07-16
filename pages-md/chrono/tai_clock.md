# std::chrono::tai_clock

```cpp
class tai_clock;  // (since C++20)
```

The clock `std::chrono::tai_clock` is a Clock that represents International
Atomic Time (TAI). It measures time since 00:00:00, 1 January 1958, and is
offset 10 seconds ahead of UTC at that date (i.e., its epoch, 1958-01-01
00:00:00 TAI, is 1957-12-31 23:59:50 UTC).

Leap seconds are not inserted into TAI. Thus, every time a leap second is
inserted into UTC, UTC falls another second behind TAI. As of December 2017, UTC
is 37 seconds behind TAI, reflecting the 10-second initial offset and the 27
leap seconds inserted between 1958 and 2017. Thus, 2018-01-01 00:00:00 UTC is
equivalent to 2018-01-01 00:00:37 TAI.

`tai_clock` meets the Clock requirements. It does not meet the TrivialClock
requirements unless the implementation can guarantee that `now()` does not throw
an exception.

### Time point family

```cpp
Defined in namespace std::chrono
template<class Duration>
using tai_time = std::chrono::time_point<std::chrono::tai_clock, Duration>;  // (since C++20)
using tai_seconds = tai_time<std::chrono::seconds>;  // (since C++20)
```

- **operator<<(std::chrono::tai_time) (C++20)** — performs stream output on a
  `tai_time` (function template)
- **from_stream(std::chrono::tai_time) (C++20)** — parses a `tai_time` from a
  stream according to the provided format (function template)
- **std::formatter<std::chrono::tai_time> (C++20)** — formatting support for
  `tai_time` (class template specialization)

### Member types

- **`rep`** — signed arithmetic type representing the number of ticks in the
  clock's duration
- **`period`** — a `std::ratio` type representing the tick period of the clock,
  in seconds
- **`duration`** — `std::chrono::duration<rep, period>`, capable of representing
  negative durations
- **`time_point`** — `std::chrono::time_point<std::chrono::tai_clock>`

### Member constants

- **constexpr bool is_steady [static]** — `true` if the time between ticks is
  always constant, i.e. calls to `now()` return values that increase
  monotonically even in case of some external clock adjustment, otherwise
  `false` (public static member constant)

### Member functions

- **now [static]** — returns a `std::chrono::time_point` representing the
  current point in time (public static member function)
- **to_utc [static]** — converts `tai_time` to `utc_time` (public static member
  function)
- **from_utc [static]** — converts `utc_time` to `tai_time` (public static
  member function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/tai_clock*
