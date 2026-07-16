# std::chrono::time_point

A `time_point<Clock, Duration>` is one moment on some `Clock`'s
timeline: internally, it's just a `Duration` counted from that clock's
epoch. `Clock::now()` is the normal way to get one; adding or
subtracting a `duration` moves it, and `time_since_epoch()` reads the
raw offset back out. (C++11)

```cpp skip
std::chrono::time_point<Clock, Duration> tp = Clock::now();  // (since C++11)
tp.time_since_epoch();                        // duration since Clock's epoch
tp + std::chrono::seconds(5);                 // move forward
std::chrono::time_point_cast<Duration2>(tp);  // change the duration type
```

### What you provide

- **Clock** — the clock this time point is measured against; must meet
  the Clock requirements (or, since C++20, be `std::chrono::local_t`,
  until C++23).
- **Duration** — a `std::chrono::duration` type for the tick; defaults
  to `Clock::duration`.

### Guarantees and costs

- Conceptually stores one `Duration` value: the offset from `Clock`'s
  epoch.
- `+=`/`-=` shift the time point in place by a duration; `++`/`--` and
  their postfix forms (C++20) step it by one tick.
- Comparisons (`== != < <= > >=`, C++11; replaced by `<=>` in C++20)
  are only meaningful between time points of the *same* `Clock` — see
  Gotchas.
- `time_point_cast` (C++11) converts to a different `Duration` on the
  same clock, truncating like `duration_cast`; `floor`, `ceil`, `round`
  (C++17) do the same conversion but round down, up, or to nearest
  instead.
- `min()`/`max()` [static] give the time points for the smallest and
  largest representable `Duration`.

### Gotchas

- Time points from different clocks are not comparable — `system_clock`
  and `steady_clock` time points have unrelated epochs, so mixing them
  is either a type mismatch (they're different `Clock` template
  arguments) or, if forced through a cast, meaningless.
- A narrowing `time_point_cast` truncates toward zero, the same
  surprise as `duration_cast` — use `floor`/`ceil`/`round` (C++17) once
  you've decided how to round.

### Example

`now()` differs on every run, so this checks properties that must hold
regardless of when it runs, instead of printing an actual time point.

```cpp
#include <chrono>
#include <iostream>

int main()
{
    using std::chrono::steady_clock;
    using std::chrono::time_point;
    using std::chrono::seconds;
    using std::chrono::milliseconds;

    time_point<steady_clock> t1 = steady_clock::now();
    time_point<steady_clock> t2 = t1 + seconds(5);

    std::cout << "t2 - t1 == 5s: " << std::boolalpha
              << (t2 - t1 == seconds(5)) << '\n';
    std::cout << "t2 > t1: " << (t2 > t1) << '\n';

    auto t3 = std::chrono::time_point_cast<milliseconds>(t1);
    auto expected = std::chrono::duration_cast<milliseconds>(t1.time_since_epoch());
    std::cout << "cast matches duration_cast: "
              << (t3.time_since_epoch() == expected) << '\n';
}
```

```text
t2 - t1 == 5s: true
t2 > t1: true
cast matches duration_cast: true
```

### Reference

```cpp skip
template<
    class Clock,
    class Duration = typename Clock::duration
> class time_point;  // (since C++11)
```

**Member types** — `clock`: `Clock`. `duration`: `Duration`. `rep`: the
duration's tick-count type. `period`: the duration's `std::ratio` tick
period.

**Member functions** — constructor; `time_since_epoch()`;
`operator+=`/`operator-=`; `operator++`/`operator++(int)`/
`operator--`/`operator--(int)` (C++20); `min()`/`max()` [static].

**Non-member functions** — `operator+`/`operator-` (C++11); comparisons
`== != < <= > >=` (C++11; individual overloads removed in C++20 in
favor of `<=>`); `time_point_cast` (C++11); `floor`, `ceil`, `round`
(all C++17).

**Helper classes** — `std::common_type<time_point>` specialization
(C++11); `std::hash<time_point>` (C++26).

### See also

- **duration** (C++11) — the interval type a `time_point` is built from
- **system_clock** (C++11) — a Clock whose time points map to calendar
  time
- **steady_clock** (C++11) — a Clock whose time points only ever
  increase
- **duration_cast** (C++11) — the equivalent truncating conversion for
  plain durations

---
*Source: https://en.cppreference.com/w/cpp/chrono/time_point*
