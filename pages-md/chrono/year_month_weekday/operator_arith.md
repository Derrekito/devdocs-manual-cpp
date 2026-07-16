# std::chrono::year_month_weekday::operator+=, std::chrono::year_month_weekday::operator-=

```cpp
constexpr std::chrono::year_month_weekday&
    operator+=( const std::chrono::years& dy ) const noexcept;  // (1) (since C++20)
constexpr std::chrono::year_month_weekday&
    operator+=( const std::chrono::months& dm ) const noexcept;  // (2) (since C++20)
constexpr std::chrono::year_month_weekday&
    operator-=( const std::chrono::years& dy ) const noexcept;  // (3) (since C++20)
constexpr std::chrono::year_month_weekday&
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
#include <iostream>

int main()
{
    auto ymwi{1/std::chrono::Wednesday[2]/2021};
    std::cout << ymwi << '\n';

    ymwi += std::chrono::years(5);
    std::cout << ymwi << '\n';
    assert(static_cast<std::chrono::year_month_day>(ymwi) ==
                       std::chrono::year(2026)/1/14);

    ymwi -= std::chrono::months(1);
    std::cout << ymwi << '\n';
    assert(static_cast<std::chrono::year_month_day>(ymwi) ==
                       std::chrono::day(10)/12/2025);
}
```

Output:

```text
2021/Jan/Wed[2]
2026/Jan/Wed[2]
2025/Dec/Wed[2]
```

### See also

- **operator+operator- (C++20)** — adds or subtracts a `year_month_weekday` and
  some number of years or months (function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/year_month_weekday/operator_arith*
