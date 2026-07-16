# std::chrono::year_month_weekday_last::operator+=, std::chrono::year_month_weekday_last::operator-=

```cpp
constexpr std::chrono::year_month_weekday_last&
    operator+=( const std::chrono::years& dy ) const noexcept;  // (1) (since C++20)
constexpr std::chrono::year_month_weekday_last&
    operator+=( const std::chrono::months& dm ) const noexcept;  // (2) (since C++20)
constexpr std::chrono::year_month_weekday_last&
    operator-=( const std::chrono::years& dy ) const noexcept;  // (3) (since C++20)
constexpr std::chrono::year_month_weekday_last&
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
#include <chrono>
#include <iostream>
using namespace std::chrono;

int main()
{
    auto ymwdl{August/Friday[last]/2022};
    std::cout << year_month_day{ymwdl} << '\n';
    ymwdl += months(2);
    std::cout << year_month_day{ymwdl} << '\n';
    ymwdl -= years(1);
    std::cout << year_month_day{ymwdl} << '\n';
}
```

Output:

```text
2022-08-26
2022-10-28
2021-10-29
```

### See also

- **operator+operator- (C++20)** — adds or subtracts a `year_month_weekday_last`
  and some number of years or months (function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/year_month_weekday_last/operator_arith*
