# std::chrono::ceil(std::chrono::duration)

```cpp
template< class ToDuration, class Rep, class Period >
constexpr ToDuration ceil( const std::chrono::duration<Rep, Period>& d );  // (since C++17)
```

Returns the smallest duration `t` representable in `ToDuration` that is greater
or equal to `d`.

The function does not participate in the overload resolution unless `ToDuration`
is a specialization of `std::chrono::duration`.

### Parameters

- **d** — duration to convert

### Return value

`d` rounded up to a duration of type `ToDuration`.

### Possible implementation

```cpp
namespace detail
{
    template<class> inline constexpr bool is_duration_v = false;
    template<class Rep, class Period> inline constexpr bool is_duration_v<
        std::chrono::duration<Rep, Period>> = true;
}

template<class To, class Rep, class Period,
         class = std::enable_if_t<detail::is_duration_v<To>>>
constexpr To ceil(const std::chrono::duration<Rep, Period>& d)
{
    To t = std::chrono::duration_cast<To>(d);
    if (t < d)
        return t + To{1};
    return t;
}
```

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    using namespace std::chrono_literals;
    using Min = std::chrono::minutes;

    std::cout
        << std::showpos
        << "ceil(+2.4min) = " << std::chrono::ceil<Min>(+2.4min).count() << "min\n"
        << "ceil(-2.4min) = " << std::chrono::ceil<Min>(-2.4min).count() << "min\n"
        << "ceil(+0.0min) = " << std::chrono::ceil<Min>(+0.0min).count() << "min\n";
}
```

Output:

```text
ceil(+2.4min) = +3min
ceil(-2.4min) = -2min
ceil(+0.0min) = +0min
```

### See also

- **duration_cast (C++11)** — converts a duration to another, with a different
  tick interval (function template)
- **floor(std::chrono::duration) (C++17)** — converts a duration to another,
  rounding down (function template)
- **round(std::chrono::duration) (C++17)** — converts a duration to another,
  rounding to nearest, ties to even (function template)
- **ceil(std::chrono::time_point) (C++17)** — converts a time_point to another,
  rounding up (function template)
- **ceilceilfceill (C++11)(C++11)** — nearest integer not less than the given
  value (function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/duration/ceil*
