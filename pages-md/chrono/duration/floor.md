# std::chrono::floor(std::chrono::duration)

```cpp
template< class ToDuration, class Rep, class Period >
constexpr ToDuration floor( const std::chrono::duration<Rep, Period>& d );  // (since C++17)
```

Returns the greatest duration `t` representable in `ToDuration` that is less or
equal to `d`.

The function does not participate in the overload resolution unless `ToDuration`
is a specialization of `std::chrono::duration`.

### Parameters

- **d** — duration to convert

### Return value

`d` rounded down to a duration of type `ToDuration`.

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
constexpr To floor(const duration<Rep, Period>& d)
{
    To t = std::chrono::duration_cast<To>(d);
    if (t > d)
        return t - To{1};
    return t;
}
```

### Example

```cpp
#include <chrono>
#include <iomanip>
#include <iostream>

int main()
{
    using namespace std::chrono_literals;
    std::cout << "Duration\tFloor\tRound\tCeil\n";
    for (using Sec = std::chrono::seconds;
        auto const d : {+4999ms, +5000ms, +5001ms, +5499ms, +5500ms, +5999ms,
                        -4999ms, -5000ms, -5001ms, -5499ms, -5500ms, -5999ms})
        std::cout << std::showpos << d << "\t\t"
                  << std::chrono::floor<Sec>(d) << '\t'
                  << std::chrono::round<Sec>(d) << '\t'
                  << std::chrono::ceil <Sec>(d) << '\n';
}
```

Output:

```text
Duration        Floor        Round        Ceil
+4999ms                +4s        +5s        +5s
+5000ms                +5s        +5s        +5s
+5001ms                +5s        +5s        +6s
+5499ms                +5s        +5s        +6s
+5500ms                +5s        +6s        +6s
+5999ms                +5s        +6s        +6s
-4999ms                -5s        -5s        -4s
-5000ms                -5s        -5s        -5s
-5001ms                -6s        -5s        -5s
-5499ms                -6s        -5s        -5s
-5500ms                -6s        -6s        -5s
-5999ms                -6s        -6s        -5s
```

### See also

- **duration_cast (C++11)** — converts a duration to another, with a different
  tick interval (function template)
- **ceil(std::chrono::duration) (C++17)** — converts a duration to another,
  rounding up (function template)
- **round(std::chrono::duration) (C++17)** — converts a duration to another,
  rounding to nearest, ties to even (function template)
- **floor(std::chrono::time_point) (C++17)** — converts a time_point to another,
  rounding down (function template)
- **floorfloorffloorl (C++11)(C++11)** — nearest integer not greater than the
  given value (function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/duration/floor*
