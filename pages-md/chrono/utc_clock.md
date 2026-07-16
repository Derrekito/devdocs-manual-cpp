# std::chrono::utc_clock

```cpp
class utc_clock;  // (since C++20)
```

The clock `std::chrono::utc_clock` is a Clock that represents Coordinated
Universal Time (UTC). It measures time since 00:00:00 UTC, Thursday, 1 January
1970, including leap seconds.

`utc_clock` meets the Clock requirements. It does not meet the TrivialClock
requirements unless the implementation can guarantee that `now()` does not throw
an exception.

### Time point family

```cpp
Defined in namespace std::chrono
template<class Duration>
using utc_time = std::chrono::time_point<std::chrono::utc_clock, Duration>;  // (since C++20)
using utc_seconds = utc_time<std::chrono::seconds>;  // (since C++20)
```

- **operator<<(std::chrono::utc_time) (C++20)** — performs stream output on a
  `utc_time` (function template)
- **from_stream(std::chrono::utc_time) (C++20)** — parses a `utc_time` from a
  stream according to the provided format (function template)
- **std::formatter<std::chrono::utc_time> (C++20)** — formatting support for
  `utc_time` (class template specialization)

### Member types

- **`rep`** — signed arithmetic type representing the number of ticks in the
  clock's duration
- **`period`** — a `std::ratio` type representing the tick period of the clock,
  in seconds
- **`duration`** — `std::chrono::duration<rep, period>`, capable of representing
  negative durations
- **`time_point`** — `std::chrono::time_point<std::chrono::utc_clock>`

### Member constants

- **constexpr bool is_steady [static]** — `true` if the time between ticks is
  always constant, i.e. calls to `now()` return values that increase
  monotonically even in case of some external clock adjustment, otherwise
  `false` (public static member constant)

### Member functions

- **now [static]** — returns a `std::chrono::time_point` representing the
  current point in time (public static member function)
- **to_sys [static]** — converts `utc_time` to `sys_time` (public static member
  function)
- **from_sys [static]** — converts `sys_time` to `utc_time` (public static
  member function)

### Non-member functions

- **get_leap_second_info (C++20)** — obtains leap second insertion information
  from a `utc_time` object (function template)

### Helper classes

- **leap_second_info (C++20)** — leap second insertion information (class)

### Notes

The official UTC epoch is 1 January 1972. `utc_clock` uses 1 January 1970
instead to be consistent with `std::chrono::system_clock`.

---
*Source: https://en.cppreference.com/w/cpp/chrono/utc_clock*
