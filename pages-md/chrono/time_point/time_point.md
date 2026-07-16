# std::chrono::time_point<Clock,Duration>::time_point

```cpp
time_point();  // (since C++11) (constexpr since C++14)
explicit time_point( const duration& d );  // (since C++11) (constexpr since C++14)
template< class Duration2 >
time_point( const time_point<Clock, Duration2>& t );  // (since C++11) (constexpr since C++14)
```

Constructs a new `time_point` from one of several optional data sources.

1) Default constructor, creates a `time_point` representing the `Clock`'s epoch
   (i.e., `time_since_epoch()` is zero).

2) Constructs a `time_point` at `Clock`'s epoch plus `d`.

3) Constructs a `time_point` by converting `t` to `duration`. This constructor
   only participates in overload resolution if `Duration2` is implicitly
   convertible to `duration`.

### Parameters

- **d** — a `duration` to copy from
- **t** — a `time_point` to convert from

### Example

```cpp
#include <chrono>
#include <iostream>

using Clock = std::chrono::steady_clock;
using TimePoint = std::chrono::time_point<Clock>;

void print_ms(const TimePoint& point)
{
    using Ms = std::chrono::milliseconds;
    const Clock::duration since_epoch = point.time_since_epoch();
    std::cout << std::chrono::duration_cast<Ms>(since_epoch) << '\n';
}

int main()
{
    const TimePoint default_value = TimePoint(); // (1)
    print_ms(default_value); // 0ms

    const Clock::duration duration_4_seconds = std::chrono::seconds(4);
    const TimePoint time_point_4_seconds(duration_4_seconds); // (2)
    // 4 seconds from start of epoch
    print_ms(time_point_4_seconds); // 4000ms

    const TimePoint time_point_now = Clock::now(); // (3)
    print_ms(time_point_now); // 212178842ms
}
```

Possible output:

```text
0ms
4000ms
212178842ms
```

### See also

- **(constructor)** — constructs new duration (public member function of
  `std::chrono::duration<Rep,Period>`)
- **duration_cast (C++11)** — converts a duration to another, with a different
  tick interval (function template)

---
*Source: https://en.cppreference.com/w/cpp/chrono/time_point/time_point*
