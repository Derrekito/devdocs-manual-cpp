# std::chrono::duration

A `duration` represents a time interval as a count of ticks (type `Rep`)
times a compile-time tick period (type `Period`, a `std::ratio` giving
the length of one tick in seconds). The only thing actually stored is
the count; `Period` lives purely in the type and is used when converting
between duration types. Converting to a coarser period can lose
precision — that conversion needs an explicit `duration_cast` (or, since
C++17, `floor`/`ceil`/`round`). Literal suffixes like `5s` or `10ms`
(C++14) construct the common duration types directly.

```cpp skip
std::chrono::duration<Rep, Period> d{n};      // Rep ticks, Period seconds each
using namespace std::chrono_literals;
auto x = 5s;                                                          // (since C++14)
auto ms = std::chrono::duration_cast<std::chrono::milliseconds>(x);  // lossy cast
```

### Guarantees and costs

- The only stored state is the tick count (`Rep`); `Period` is a
  compile-time tag, not runtime data.
- Implicit construction from another duration is only allowed when it
  can't lose precision (e.g. seconds to milliseconds); the lossy
  direction (milliseconds to seconds) requires an explicit
  `duration_cast`, which truncates toward zero.
- `floor`, `ceil`, and `round` (C++17) perform the lossy conversion by
  rounding down, up, or to nearest (ties to even) instead of truncating.
  `abs` (C++17) gives a duration's absolute value.
- The predefined aliases `nanoseconds` through `hours` each guarantee a
  representable range of at least ±292 years; `days`, `weeks`, `months`,
  and `years` (all C++20) guarantee at least ±40000 years. `years` is
  365.2425 days (the average Gregorian year); `months` is 1/12 of that.

### Gotchas

- A narrowing implicit conversion between duration types is a compile
  error, not a silent truncation — that's intentional; use
  `duration_cast` (or `floor`/`ceil`/`round`, C++17) once you've decided
  how to round.
- The C++20 literal suffixes `d` and `y` name the singular calendar types
  `day` and `year`, not `std::chrono::days`/`std::chrono::years` — don't
  confuse them.
- Combining two durations with different `Period`s works via
  `std::common_type`, but the result's period isn't necessarily either
  operand's — check the result type, don't assume.

### Example

```cpp
#include <chrono>
#include <iostream>
#include <ratio>

int main()
{
    using namespace std::chrono_literals;

    auto d = 1s;
    std::cout << std::chrono::milliseconds(d).count() << " ms\n";
    std::cout << std::chrono::duration_cast<std::chrono::minutes>(d).count()
              << " min (truncated)\n";

    std::chrono::duration<double, std::ratio<1, 24>> fps_24{d};
    std::cout << fps_24.count() << " frames at 24fps\n";
}
```

```text
1000 ms
0 min (truncated)
24 frames at 24fps
```

### Reference

```cpp skip
template<
    class Rep,
    class Period = std::ratio<1>
> class duration;  // (since C++11)
```

**Template parameters** — `Rep`: the arithmetic type storing the tick
count (use a floating-point type for fractional ticks). `Period`:
`std::ratio<Num, Denom>` giving one tick's length in seconds; defaults to
`std::ratio<1>` (whole seconds).

**Member types** — `rep` is `Rep`; `period` is `Period` (until C++17) /
`typename Period::type` (since C++17), a `std::ratio`.

**Member functions** — constructor, `operator=`, `count()`,
`zero()`/`min()`/`max()` (static), unary `+`/`-`, pre/post `++`/`--`, and
compound assignment `+= -= *= /= %=`.

**Non-member functions** — arithmetic `+ - * / %` (C++11); comparisons
`== != < <= > >=` (C++11), with `<=>` replacing the individual relational
operators (C++20); `duration_cast` (C++11, truncating conversion);
`floor`, `ceil`, `round`, `abs` (all C++17); `operator<<` and
`from_stream` (both C++20).

**Predefined aliases** — `nanoseconds`, `microseconds`, `milliseconds`,
`seconds`, `minutes`, `hours` (C++11); `days`, `weeks`, `months`, `years`
(C++20).

**Literal suffixes** (C++14, in `std::chrono_literals`) — `h`, `min`,
`s`, `ms`, `us`, `ns`.

**Helper classes** — `std::common_type<duration>` specialization
(C++11); `treat_as_floating_point` (C++11); `duration_values` (C++11);
`std::formatter<duration>` (C++20); `std::hash<duration>` (C++26).

Feature-test macro `__cpp_lib_chrono_udls` (value `201304L`, C++14)
signals literal-suffix support.

### See also

- **duration_cast** (C++11) — converts a duration to another, with a
  different tick period
- **time_point** — a point in time, expressed as a duration since a
  clock's epoch
- **system_clock** — a clock whose `now()` produces `time_point`s usable
  with `duration`
- **treat_as_floating_point** (C++11) — marks a `Rep` as convertible
  between durations with different periods

---
*Source: https://en.cppreference.com/w/cpp/chrono/duration*
