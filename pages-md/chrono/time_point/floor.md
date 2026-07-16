# std::chrono::floor(std::chrono::time_point)

```cpp
template< class ToDuration, class Clock, class Duration >
constexpr std::chrono::time_point<Clock, ToDuration>
    floor( const std::chrono::time_point<Clock, Duration>& tp );  // (since C++17)
```

Returns the largest time point `t` representable in `ToDuration` that is smaller
or equal to `tp`.

The function does not participate in the overload resolution unless `ToDuration`
is a specialization of `std::chrono::duration`.

### Parameters

- **tp** — time point to round down

### Return value

`tp` rounded down to the next time point using duration of type `ToDuration`.

### Possible implementation

```cpp
namespace detail
{
    template<class> inline constexpr bool is_duration_v = false;
    template<class Rep, class Period> inline constexpr bool is_duration_v<
        std::chrono::duration<Rep, Period>> = true;
}

template<class To, class Clock, class FromDuration,
         class = std::enable_if_t<detail::is_duration_v<To>>>
constexpr std::chrono::time_point<Clock, To>
    floor(const std::chrono::time_point<Clock, FromDuration>& tp)
{
    return std::chrono::time_point<Clock, To>{
        std::chrono::floor<To>(tp.time_since_epoch())};
}
```

### Example

```cpp
#include <chrono>
#include <iostream>
#include <string>

template<typename TimePoint>
std::string to_string(const TimePoint& time_point)
{
    return std::to_string(time_point.time_since_epoch().count());
}

int main()
{
    using namespace std::literals::chrono_literals;
    using Sec = std::chrono::seconds;

    std::cout << "Time point\t" "Cast\t" "Floor\t" "Round\t" "Ceil\n";
    std::cout << "(ms)\t\t"     "(s)\t"  "(s)\t"   "(s)\t"   "(s)\n";
    for (const auto value_ms : {5432ms, 5678ms})
    {
        std::chrono::time_point<std::chrono::system_clock, std::chrono::milliseconds>
            time_point_ms(value_ms);

        std::cout
            << to_string(time_point_ms) << "\t\t"
            << to_string(std::chrono::time_point_cast<Sec>(time_point_ms)) << '\t'
            << to_string(std::chrono::floor<Sec>(time_point_ms)) << '\t'
            << to_string(std::chrono::round<Sec>(time_point_ms)) << '\t'
            << to_string(std::chrono::ceil<Sec>(time_point_ms)) << '\n';
    }
}
```

Output:

```text
Time point        Cast        Floor        Round        Ceil
(ms)                (s)        (s)        (s)        (s)
5432                5        5        5        6
5678                5        5        6        6
```

### See also

- **time_point_cast (C++11)** — converts a time point to another time point on
  the same clock, with a different duration (function template)
- **ceil(std::chrono::time_point) (C++17)** — converts a time_point to another,
  rounding up (function template)
- **round(std::chrono::time_point) (C++17)** — converts a time_point to another,
  rounding to nearest, ties to even (function template)
- **floor(std::chrono::duration) (C++17)** — converts a duration to another,
  rounding down (function template)
- **floorfloorffloorl (C++11)(C++11)** — nearest integer not greater than the
  given value (function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/time_point/floor*
