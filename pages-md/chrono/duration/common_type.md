# std::common_type<std::chrono::duration>

```cpp
template< class Rep1, class Period1, class Rep2, class Period2 >
struct common_type<std::chrono::duration<Rep1, Period1>,
                   std::chrono::duration<Rep2, Period2>> {
    typedef std::chrono::duration<
        typename std::common_type<Rep1, Rep2>::type, /*see note*/> type;
};  // (since C++11)
```

Exposes the type named `type`, which is the common type of two
`std::chrono::duration`s, whose period is the greatest common divisor of
`Period1` and `Period2`.

### Note

The period of the resulting duration can be computed by forming a ratio of the
greatest common divisor of `Period1::num` and `Period2::num` and the least
common multiple of `Period1::den` and `Period2::den`.

### Example

```cpp
#include <chrono>
#include <iostream>
#include <type_traits>

// std::chrono already finds the greatest common divisor,
// likely using std::common_type<>. We make the type
// deduction externally.

template<typename T,typename S>
constexpr auto durationDiff(const T& t, const S& s)
    -> typename std::common_type<T,S>::type
{
    typedef typename std::common_type<T,S>::type Common;
    return Common(t) - Common(s);
}

int main()
{
    using namespace std::literals;

    constexpr auto ms = 30ms;
    constexpr auto us = 1100us;
    constexpr auto diff = durationDiff(ms, us);

    std::cout << ms << " - " << us << " = " << diff << '\n';
}
```

Output:

```text
30ms - 1100us = 28900us
```

### See also

- **std::common_type<std::chrono::time_point> (C++11)** — specializes the
  `std::common_type` trait (class template specialization)
- **common_type (C++11)** — determines the common type of a group of types
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/chrono/duration/common_type*
