# std::chrono::system_clock

The system-wide wall-clock time — what you want to display a timestamp
or convert to/from calendar time (`time_t`). It's the only standard
clock with a `to_time_t`/`from_time_t` bridge to C time. It is not
necessarily monotonic: the OS can step it backward or forward (NTP sync,
a manual clock change), so never use it to measure elapsed time — use
`std::chrono::steady_clock` for that. (C++11)

```cpp skip
auto now = std::chrono::system_clock::now();                 // (since C++11)
std::time_t t = std::chrono::system_clock::to_time_t(now);
auto tp = std::chrono::system_clock::from_time_t(t);
std::chrono::sys_seconds s = std::chrono::floor<std::chrono::seconds>(now);  // (since C++20)
```

### What it gives you

- Member types `rep`, `period`, `duration`, `time_point` — standard
  Clock machinery; `duration` is `std::chrono::duration<rep, period>`
  and `time_point` is `std::chrono::time_point<system_clock>`.
- `is_steady` — `false` on essentially every real system: `now()` isn't
  guaranteed to move only forward.

### Guarantees and costs

- Meets the TrivialClock requirements.
- Epoch: unspecified before C++20 (most implementations already used
  Unix Time — since 00:00:00 UTC, 1 January 1970, not counting leap
  seconds — but it wasn't guaranteed); since C++20 the epoch is defined
  to be Unix Time.
- `to_time_t()`/`from_time_t()` are the only portable bridge to
  `<ctime>` facilities like `std::localtime`/`std::put_time`.
- Since C++20, `sys_time<Duration>` names
  `time_point<system_clock, Duration>`, with aliases `sys_seconds` and
  `sys_days`, plus `operator<<`, `from_stream`, and a `std::formatter`
  specialization for it.
- Daylight Saving Time and time-zone changes do not move it — it's
  UTC-based; only an explicit clock adjustment (e.g. NTP sync) does.

### Gotchas

- Never use it to time an operation: the OS can step it at any moment,
  so `end - start` can be wrong, even negative. Use `steady_clock` for
  measuring intervals.
- `is_steady` is `false` — don't assume consecutive `now()` calls only
  move forward.

### Example

`now()` and its `to_time_t` value are different on every run, so this
checks a deterministic fact — the round trip through `time_t` — instead
of printing the timestamp itself.

```cpp
#include <chrono>
#include <ctime>
#include <iostream>

int main()
{
    using std::chrono::system_clock;
    using std::chrono::seconds;

    auto now = system_clock::now();
    std::time_t t = system_clock::to_time_t(now);
    auto back = system_clock::from_time_t(t);

    auto diff = now - back;
    bool close = diff < seconds(1) && diff > seconds(-1);

    std::cout << "to_time_t round trip within 1s: " << std::boolalpha
              << close << '\n';
    std::cout << "is_steady: " << system_clock::is_steady << '\n';
}
```

```text
to_time_t round trip within 1s: true
is_steady: false
```

### Reference

```cpp skip
class system_clock;  // (since C++11)

template<class Duration>
using sys_time = std::chrono::time_point<std::chrono::system_clock, Duration>;  // (since C++20)
using sys_seconds = sys_time<std::chrono::seconds>;  // (since C++20)
using sys_days = sys_time<std::chrono::days>;  // (since C++20)
```

**Member types** — `rep`: signed arithmetic tick-count type. `period`:
`std::ratio` tick period, in seconds. `duration`:
`std::chrono::duration<rep, period>`, able to represent negative
durations. `time_point`: `std::chrono::time_point<system_clock>`.

**Member constants** — `is_steady` (static constexpr bool).

**Member functions** — `now()` [static]; `to_time_t()` [static]
(converts to `std::time_t`); `from_time_t()` [static] (converts from
`std::time_t`).

**`sys_time` support (C++20)** — `operator<<`, `from_stream`, and a
`std::formatter<sys_time>` specialization.

### See also

- **steady_clock** (C++11) — monotonic clock, for measuring intervals
- **high_resolution_clock** (C++11) — the clock with the shortest tick
  period available
- **time_point** (C++11) — the type `now()` returns
- **duration** (C++11) — the interval type a `time_point` is built from

---
*Source: https://en.cppreference.com/w/cpp/chrono/system_clock*
