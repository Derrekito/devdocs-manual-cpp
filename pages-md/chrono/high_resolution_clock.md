# std::chrono::high_resolution_clock

```cpp
class high_resolution_clock;  // (since C++11)
```

Class `std::chrono::high_resolution_clock` represents the clock with the
smallest tick period provided by the implementation. It may be an alias of
`std::chrono::system_clock` or `std::chrono::steady_clock`, or a third,
independent clock.

`std::chrono::high_resolution_clock` meets the requirements of TrivialClock.

### Member types

- **`rep`** — arithmetic type representing the number of ticks in the clock's
  duration
- **`period`** — a `std::ratio` type representing the tick period of the clock,
  in seconds
- **`duration`** — `std::chrono::duration<rep, period>`
- **`time_point`** —
  `std::chrono::time_point<std::chrono::high_resolution_clock>`

### Member constants

- **constexpr bool is_steady [static]** — `true` if the time between ticks is
  always constant, i.e. calls to `now()` return values that increase
  monotonically even in case of some external clock adjustment, otherwise
  `false` (public static member constant)

### Member functions

- **now [static]** — returns a `std::chrono::time_point` representing the
  current value of the clock (public static member function)

### Notes

There has been some controversy around the use of `high_resolution_clock`.
Howard Hinnant, who claims to have introduced `high_resolution_clock` to the
language, stated in 2016 on the ISO C++ Standard - Discussion mailing list that
he was in favor of deprecating it. His rationale was that, because the standard
allows for it to be an alias for `std::chrono::steady_clock` or
`std::chrono::system_clock`, its use adds uncertainty to a program without
benefit. However, other participants in the thread spoke out its favor, for
instance on the basis that, because neither `std::chrono::steady_clock` nor
`std::chrono::system_clock` come with any particular resolution guarantees,
`high_resolution_clock` serves a useful role by giving the vendor an opportunity
to supply the platform's highest-resolution clock, when neither its
`std::chrono::steady_clock` nor its `std::chrono::system_clock` would be that.

It is often just an alias for `std::chrono::steady_clock` or
`std::chrono::system_clock`, but which one it is depends on the library or
configuration. When it is a `system_clock`, it is not monotonic (e.g., the time
can go backwards). For example, as of 2023, libstdc++ has it aliased to
`system_clock` "until higher-than-nanosecond definitions become feasible"[1],
MSVC has it as `steady_clock`[2], and libc++ uses `steady_clock` when the C++
standard library implementation supports a monotonic clock and `system_clock`
otherwise[3].

### See also

- **system_clock (C++11)** — wall clock time from the system-wide realtime clock
  (class)
- **steady_clock (C++11)** — monotonic clock that will never be adjusted (class)

### External links

  libstdc++ <chrono.h> msvc high_resolution_clock libc++
      <high_resolution_clock.h>

---
*Source: https://en.cppreference.com/w/cpp/chrono/high_resolution_clock*
