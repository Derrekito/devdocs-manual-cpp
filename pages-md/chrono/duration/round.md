# std::chrono::round(std::chrono::duration)

```cpp
template< class ToDuration, class Rep, class Period >
constexpr ToDuration round( const std::chrono::duration<Rep, Period>& d );  // (since C++17)
```

Returns the value `t` representable in `ToDuration` that is the closest to `d`.
If there are two such values, returns the even value (that is, the value `t`
such that `t % 2 == 0`).

The function does not participate in the overload resolution unless `ToDuration`
is a specialization of `std::chrono::duration` and
`std::chrono::treat_as_floating_point_v<typename ToDuration::rep>` is `false`.

### Parameters

- **d** — duration to convert

### Return value

`d` rounded to the nearest duration of type `ToDuration`, rounding to even in
halfway cases.

### Possible implementation

```cpp
namespace detail
{
    template<class> inline constexpr bool is_duration_v = false;
    template<class Rep, class Period> inline constexpr bool is_duration_v<
        std::chrono::duration<Rep, Period>> = true;
}

template<class To, class Rep, class Period,
         class = std::enable_if_t<detail::is_duration_v<To> &&
                !std::chrono::treat_as_floating_point_v<typename To::rep>>>
constexpr To round(const std::chrono::duration<Rep, Period>& d)
{
    To t0 = std::chrono::floor<To>(d);
    To t1 = t0 + To{1};
    auto diff0 = d - t0;
    auto diff1 = t1 - d;
    if (diff0 == diff1)
    {
        if (t0.count() & 1)
            return t1;
        return t0;
    }
    else if (diff0 < diff1)
        return t0;
    return t1;
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
- **floor(std::chrono::duration) (C++17)** — converts a duration to another,
  rounding down (function template)
- **ceil(std::chrono::duration) (C++17)** — converts a duration to another,
  rounding up (function template)
- **round(std::chrono::time_point) (C++17)** — converts a time_point to another,
  rounding to nearest, ties to even (function template)
- **roundroundfroundllroundlroundflroundlllroundllroundfllroundl
  (C++11)(C++11)(C++11)(C++11)(C++11)(C++11)(C++11)(C++11)(C++11)** — nearest
  integer, rounding away from zero in halfway cases (function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/duration/round*
