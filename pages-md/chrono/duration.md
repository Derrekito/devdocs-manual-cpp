# std::chrono::duration

```cpp
template<
    class Rep,
    class Period = std::ratio<1>
> class duration;  // (since C++11)
```

Class template `std::chrono::duration` represents a time interval.

It consists of a count of ticks of type `Rep` and a tick period, where the tick
period is a compile-time rational `fraction` representing the time in seconds
from one tick to the next.

The only data stored in a `duration` is a tick count of type `Rep`. If `Rep` is
floating point, then the `duration` can represent fractions of ticks. `Period`
is included as part of the duration's type, and is only used when converting
between different durations.

### Member types

- **`rep`** — `Rep`, an arithmetic type representing the number of ticks
- **`period`** — `Period`(until C++17)`typename Period::type`(since C++17), a
  `std::ratio` representing the tick period (i.e. the number of second's
  fractions per tick)

### Member functions

- **(constructor)** — constructs new duration (public member function)
- **operator=** — assigns the contents (public member function)
- **count** — returns the count of ticks (public member function)
- **zero [static]** — returns the special duration value zero (public static
  member function)
- **min [static]** — returns the special duration value min (public static
  member function)
- **max [static]** — returns the special duration value max (public static
  member function)
- **operator+operator-** — implements unary + and unary - (public member
  function)
- **operator++operator++(int)operator--operator--(int)** — increments or
  decrements the tick count (public member function)
- **operator+=operator-=operator*=operator/=operator%=** — implements compound
  assignment between two durations (public member function)

### Non-member functions

- **operator+operator-operator*operator/operator% (C++11)** — implements
  arithmetic operations with durations as arguments (function template)
- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (C++11)(C++11)(removed in C++20)(C++11)(C++11)(C++11)(C++11)(C++20)** —
  compares two durations (function template)
- **duration_cast (C++11)** — converts a duration to another, with a different
  tick interval (function template)
- **floor(std::chrono::duration) (C++17)** — converts a duration to another,
  rounding down (function template)
- **ceil(std::chrono::duration) (C++17)** — converts a duration to another,
  rounding up (function template)
- **round(std::chrono::duration) (C++17)** — converts a duration to another,
  rounding to nearest, ties to even (function template)
- **abs(std::chrono::duration) (C++17)** — obtains the absolute value of the
  duration (function template)
- **operator<< (C++20)** — performs stream output on a `duration` (function
  template)
- **from_stream (C++20)** — parses a `duration` from a stream according to the
  provided format (function template)

### Helper types

A type /* intXX */ used in the table below means a signed integer type of at
least XX bits.

- **`std::chrono::nanoseconds`** — std::chrono::duration</* int64 */, std::nano>
- **`std::chrono::microseconds`** — std::chrono::duration</* int55 */,
  std::micro>
- **`std::chrono::milliseconds`** — std::chrono::duration</* int45 */,
  std::milli>
- **`std::chrono::seconds`** — std::chrono::duration</* int35 */>
- **`std::chrono::minutes`** — std::chrono::duration</* int29 */,
  std::ratio<60>>
- **`std::chrono::hours`** — std::chrono::duration</* int23 */,
  std::ratio<3600>>
- **`std::chrono::days` (since C++20)** — std::chrono::duration</* int25 */,
  std::ratio<86400>>
- **`std::chrono::weeks` (since C++20)** — std::chrono::duration</* int22 */,
  std::ratio<604800>>
- **`std::chrono::months` (since C++20)** — std::chrono::duration</* int20 */,
  std::ratio<2629746>>
- **`std::chrono::years` (since C++20)** — std::chrono::duration</* int17 */,
  std::ratio<31556952>>

Note: each of the predefined duration types up to `hours` covers a range of at
least ±292 years.

Each of the predefined duration types `days`, `weeks`, `months` and `years`
covers a range of at least ±40000 years. `years` is equal to 365.2425 `days`
(the average length of a Gregorian year). `months` is equal to 30.436875 `days`
(exactly 1/12 of `years`).
*(since C++20)*

### Helper classes

- **std::common_type<std::chrono::duration> (C++11)** — specializes the
  `std::common_type` trait (class template specialization)
- **treat_as_floating_point (C++11)** — indicates that a duration is convertible
  to duration with different tick period (class template)
- **duration_values (C++11)** — constructs zero, min, and max values of a tick
  count of given type (class template)
- **std::formatter<std::chrono::duration> (C++20)** — formatting support for
  `duration` (class template specialization)
- **std::hash<std::chrono::duration> (C++26)** — hash support for
  **`std::chrono::duration`** (class template specialization)

### Literals

- **operator""h (C++14)** — a `std::chrono::duration` literal representing hours
  (function)
- **operator""min (C++14)** — a `std::chrono::duration` literal representing
  minutes (function)
- **operator""s (C++14)** — a `std::chrono::duration` literal representing
  seconds (function)
- **operator""ms (C++14)** — a `std::chrono::duration` literal representing
  milliseconds (function)
- **operator""us (C++14)** — a `std::chrono::duration` literal representing
  microseconds (function)
- **operator""ns (C++14)** — a `std::chrono::duration` literal representing
  nanoseconds (function)

Note: the literal suffixes `d` and `y` do not refer to `days` and `years` but to
`day` and `year`, respectively.
*(since C++20)*

### Notes

The actual time interval (in seconds) that is held by a duration object `d` is
roughly equal to `d.count() * D::period::num / D::period::den`, where `D` is of
type `chrono::duration<>` and `d` is an object of such type.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_chrono_udls` | 201304L | (C++14) | User-defined literals for time
      types

### Example

This example shows how to define several custom duration types and convert
between types:

```cpp
#include <chrono>
#include <iostream>

using namespace std::chrono_literals;

template<typename T1, typename T2>
using mul = std::ratio_multiply<T1, T2>;

int main()
{
    using microfortnights = std::chrono::duration<float,
        mul<mul<std::ratio<2>, std::chrono::weeks::period>, std::micro>>;
    using nanocenturies = std::chrono::duration<float,
        mul<mul<std::hecto, std::chrono::years::period>, std::nano>>;
    using fps_24 = std::chrono::duration<double, std::ratio<1, 24>>;

    std::cout << "1 second is:\n";

    // integer scale conversion with no precision loss: no cast
    std::cout << std::chrono::milliseconds(1s).count() << " milliseconds\n"
              << std::chrono::microseconds(1s).count() << " microseconds\n"
              << std::chrono::nanoseconds(1s).count() << " nanoseconds\n";

    // integer scale conversion with precision loss: requires a cast
    std::cout << std::chrono::duration_cast<std::chrono::minutes>(1s).count()
              << " minutes\n";

    // floating-point scale conversion: no cast
    std::cout << microfortnights(1s).count() << " microfortnights\n"
              << nanocenturies(1s).count() << " nanocenturies\n"
              << fps_24(1s).count() << " frames at 24fps\n";
}
```

Output:

```text
1 second is:
1000 milliseconds
1000000 microseconds
1000000000 nanoseconds
0 minutes
0.82672 microfortnights
0.316887 nanocenturies
24 frames at 24fps
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/duration*
