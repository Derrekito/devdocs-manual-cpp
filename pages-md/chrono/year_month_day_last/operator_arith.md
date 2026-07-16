# std::chrono::year_month_day_last::operator+=, std::chrono::year_month_day_last::operator-=

```cpp
constexpr std::chrono::year_month_day_last&
    operator+=( const std::chrono::years& dy ) const noexcept;  // (1) (since C++20)
constexpr std::chrono::year_month_day_last&
    operator+=( const std::chrono::months& dm ) const noexcept;  // (2) (since C++20)
constexpr std::chrono::year_month_day_last&
    operator-=( const std::chrono::years& dy ) const noexcept;  // (3) (since C++20)
constexpr std::chrono::year_month_day_last&
    operator-=( const std::chrono::months& dm ) const noexcept;  // (4) (since C++20)
```

Modifies the time point `*this` represents by the duration `dy` or `dm`.

1) Equivalent to `*this = *this + dy;`.

2) Equivalent to `*this = *this + dm;`.

3) Equivalent to `*this = *this - dy;`.

4) Equivalent to `*this = *this - dm;`.

For durations that are convertible to both `std::chrono::years` and
`std::chrono::months`, the `years` overloads (1,3) are preferred if the call
would otherwise be ambiguous.

### Example

```cpp
#include <cassert>
#include <chrono>

int main()
{
    auto ymdl{11/std::chrono::last/2020};

    ymdl += std::chrono::years(15);
    assert(ymdl.day() == std::chrono::day(30));
    assert(ymdl.month() == std::chrono::November);
    assert(ymdl.year() == std::chrono::year(2035));

    ymdl -= std::chrono::months(6);
    assert(ymdl.day() == std::chrono::day(31));
    assert(ymdl.month() == std::chrono::May);
    assert(ymdl.year() == std::chrono::year(2035));
}
```

### See also

- **operator+operator- (C++20)** — adds or subtracts a `year_month_day_last` and
  some number of years or months (function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/year_month_day_last/operator_arith*
