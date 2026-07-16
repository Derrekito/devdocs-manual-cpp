# std::chrono::steady_clock

The clock to reach for whenever you're measuring elapsed time. Its time
points never decrease and the time between ticks is constant, no matter
what the wall clock or the OS is doing to it. It isn't tied to calendar
time — it might count time since boot — so don't use it to display a
timestamp; that's `system_clock`'s job. (C++11)

```cpp skip
auto start = std::chrono::steady_clock::now();   // (since C++11)
// ... work ...
auto elapsed = std::chrono::steady_clock::now() - start;
```

### What it gives you

- Member types `rep`, `period`, `duration`, `time_point` — standard
  Clock machinery; `duration` is `std::chrono::duration<rep, period>`
  and `time_point` is `std::chrono::time_point<steady_clock>`.
- `is_steady` — always `true`. That's this clock's entire purpose.

### Guarantees and costs

- Meets the TrivialClock requirements.
- `now()` is guaranteed non-decreasing: if `t2` is captured after `t1`,
  `t2 >= t1` always holds, even across external clock adjustments.
- The time between ticks is constant.
- Not related to wall-clock time; its epoch is unspecified (for
  example, it may measure time since system boot).

### Gotchas

- There's no `to_time_t` on this clock, and its epoch has no defined
  relation to Unix time — don't try to turn its time points into
  calendar dates.
- If two processes need to agree on "what time it is" (a timestamp,
  a deadline in wall-clock terms), this isn't the clock — use
  `system_clock`.

### Example

The elapsed duration itself varies run to run, so this checks the
guarantee that matters — monotonicity — instead of printing a duration.

```cpp
#include <chrono>
#include <iostream>
#include <thread>

int main()
{
    using std::chrono::steady_clock;

    auto start = steady_clock::now();
    std::this_thread::sleep_for(std::chrono::milliseconds(1));
    auto end = steady_clock::now();

    std::cout << "monotonic (end >= start): " << std::boolalpha
              << (end >= start) << '\n';
    std::cout << "is_steady: " << steady_clock::is_steady << '\n';
}
```

```text
monotonic (end >= start): true
is_steady: true
```

### Reference

```cpp skip
class steady_clock;  // (since C++11)
```

**Member types** — `rep`: arithmetic tick-count type. `period`:
`std::ratio` tick period, in seconds. `duration`:
`std::chrono::duration<rep, period>`. `time_point`:
`std::chrono::time_point<steady_clock>`.

**Member constants** — `is_steady` (static constexpr bool, always
`true`).

**Member functions** — `now()` [static] — returns the current
`time_point`.

### See also

- **system_clock** (C++11) — wall-clock time, for timestamps rather
  than intervals
- **high_resolution_clock** (C++11) — the clock with the shortest tick
  period available
- **duration** (C++11) — the type elapsed time is expressed as
- **time_point** (C++11) — what `now()` returns

---
*Source: https://en.cppreference.com/w/cpp/chrono/steady_clock*
