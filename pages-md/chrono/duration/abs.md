# std::chrono::abs(std::chrono::duration)

```cpp
template< class Rep, class Period >
constexpr std::chrono::duration<Rep, Period> abs( std::chrono::duration<Rep, Period> d );  // (since C++17)
```

Returns the absolute value of the duration `d`. Specifically, if `d >=
d.zero()`, return `d`, otherwise return `-d`.

The function does not participate in the overload resolution unless
`std::numeric_limits<Rep>::is_signed` is `true`.

### Parameters

- **d** — duration

### Return value

Absolute value of `d`.

### Possible implementation

```cpp
template<class Rep, class Period,
    class = std::enable_if_t<std::numeric_limits<Rep>::is_signed>>
constexpr std::chrono::duration<Rep, Period> abs(std::chrono::duration<Rep, Period> d)
{
    return d >= d.zero() ? +d : -d;
}
```

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    using namespace std::chrono;

    static_assert(abs(-42s) == std::chrono::abs(42s));

    std::cout << "abs(+3min) = " << abs(3min).count() << '\n'
              << "abs(-3min) = " << abs(-3min).count() << '\n';
}
```

Output:

```text
abs(+3min) = 3
abs(-3min) = 3
```

### See also

- **operator+operator-** — implements unary + and unary - (public member
  function)
- **abs(int)labsllabs (C++11)** — computes absolute value of an integral value
  (\(\small{|x|}\)|x|) (function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/duration/abs*
