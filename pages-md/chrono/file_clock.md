# std::chrono::file_clock

```cpp
using file_clock = /* see below */;  // (since C++20)
```

`std::chrono::file_clock` is an alias for the clock used for
`std::filesystem::file_time_type`. Its epoch is unspecified.

`file_clock` meets the TrivialClock requirements.

### Time point family

```cpp
Defined in namespace std::chrono
template<class Duration>
using file_time = std::chrono::time_point<std::chrono::file_clock, Duration>;  // (since C++20)
```

- **operator<<(std::chrono::file_time) (C++20)** — performs stream output on a
  `file_time` (function template)
- **from_stream(std::chrono::file_time) (C++20)** — parses a `file_time` from a
  stream according to the provided format (function template)
- **std::formatter<std::chrono::file_time> (C++20)** — formatting support for
  `file_time` (class template specialization)

### Member types

- **`rep`** — signed arithmetic type representing the number of ticks in the
  clock's duration
- **`period`** — a `std::ratio` type representing the tick period of the clock,
  in seconds
- **`duration`** — `std::chrono::duration<rep, period>`, capable of representing
  negative durations
- **`time_point`** — `std::chrono::time_point<std::chrono::file_clock>`

### Member constants

- **constexpr bool is_steady [static]** — `true` if the time between ticks is
  always constant, i.e. calls to `now()` return values that increase
  monotonically even in case of some external clock adjustment, otherwise
  `false` (public static member constant)

### Member functions

`file_clock` provides exactly one of the following two pairs of static member
functions:

- `to_utc` and `from_utc`; or
- `to_sys` and `from_sys`.

- **now [static]** — returns a `std::chrono::time_point` representing the
  current point in time (public static member function)
- **to_utcfrom_utc [static] (optional)** — converts between `file_time` and
  `utc_time` (public static member function)
- **to_sysfrom_sys [static] (optional)** — converts between `file_time` and
  `sys_time` (public static member function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/file_clock*
