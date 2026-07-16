# std::chrono::duration_cast

```cpp
template< class ToDuration, class Rep, class Period >
constexpr ToDuration duration_cast( const std::chrono::duration<Rep,Period>& d );  // (since C++11)
```

Converts a `std::chrono::duration` to a duration of different type `ToDuration`.

The function does not participate in overload resolution unless `ToDuration` is
a specialization of `std::chrono::duration`.

No implicit conversions are used. Multiplications and divisions are avoided
where possible, if it is known at compile time that one or more parameters are
`1`. Computations are done in the widest type available and converted, as if by
`static_cast`, to the result type only when finished.

### Parameters

- **d** — duration to convert

### Return value

`d` converted to a duration of type `ToDuration`.

### Notes

Casting between integer durations where the source period is exactly divisible
by the target period (e.g. hours to minutes) or between floating-point durations
can be performed with ordinary casts or implicitly via `std::chrono::duration`
constructors, no `duration_cast` is needed.

Casting from a floating-point duration to an integer duration is subject to
undefined behavior when the floating-point value is NaN, infinity, or too large
to be representable by the target's integer type. Otherwise, casting to an
integer duration is subject to truncation as with any `static_cast` to an
integer type.

### Example

This example measures the execution time of a function.

```cpp
#include <chrono>
#include <iostream>
#include <ratio>
#include <thread>

void f()
{
    std::this_thread::sleep_for(std::chrono::seconds(1));
}

int main()
{
    const auto t1 = std::chrono::high_resolution_clock::now();
    f();
    const auto t2 = std::chrono::high_resolution_clock::now();

    // floating-point duration: no duration_cast needed
    const std::chrono::duration<double, std::milli> fp_ms = t2 - t1;

    // integral duration: requires duration_cast
    const auto int_ms = std::chrono::duration_cast<std::chrono::milliseconds>(t2 - t1);

    // converting integral duration to integral duration of
    // shorter divisible time unit: no duration_cast needed
    const std::chrono::duration<long, std::micro> int_usec = int_ms;

    std::cout << "f() took " << fp_ms << ", or "
              << int_ms << " (whole milliseconds), or "
              << int_usec << " (whole microseconds)\n";
}
```

Possible output:

```text
f() took 1000.14ms, or 1000ms (whole milliseconds), or 1000000us (whole microseconds)
```

### See also

- **duration (C++11)** — a time interval (class template)
- **time_point_cast (C++11)** — converts a time point to another time point on
  the same clock, with a different duration (function template)
- **floor(std::chrono::duration) (C++17)** — converts a duration to another,
  rounding down (function template)
- **ceil(std::chrono::duration) (C++17)** — converts a duration to another,
  rounding up (function template)
- **round(std::chrono::duration) (C++17)** — converts a duration to another,
  rounding to nearest, ties to even (function template)

---
*Source: https://en.cppreference.com/w/cpp/chrono/duration/duration_cast*
