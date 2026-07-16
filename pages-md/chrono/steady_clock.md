# std::chrono::steady_clock

```cpp
class steady_clock;  // (since C++11)
```

Class `std::chrono::steady_clock` represents a monotonic clock. The time points
of this clock cannot decrease as physical time moves forward and the time
between ticks of this clock is constant. This clock is not related to wall clock
time (for example, it can be time since last reboot), and is most suitable for
measuring intervals.

`std::chrono::steady_clock` meets the requirements of TrivialClock.

### Member types

- **`rep`** — arithmetic type representing the number of ticks in the clock's
  duration
- **`period`** — a `std::ratio` type representing the tick period of the clock,
  in seconds
- **`duration`** — `std::chrono::duration<rep, period>`
- **`time_point`** — `std::chrono::time_point<std::chrono::steady_clock>`

### Member constants

- **constexpr bool is_steady [static]** — steady clock flag, always `true`
  (public static member constant)

### Member functions

- **now [static]** — returns a time_point representing the current value of the
  clock (public static member function)

### See also

- **system_clock (C++11)** — wall clock time from the system-wide realtime clock
  (class)
- **high_resolution_clock (C++11)** — the clock with the shortest tick period
  available (class)

---
*Source: https://en.cppreference.com/w/cpp/chrono/steady_clock*
