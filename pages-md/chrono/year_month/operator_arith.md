# std::chrono::year_month::operator+=, std::chrono::year_month::operator-=

```cpp
constexpr std::chrono::year_month&
    operator+=( const std::chrono::years& dy ) const noexcept;  // (1) (since C++20)
constexpr std::chrono::year_month&
    operator+=( const std::chrono::months& dm ) const noexcept;  // (2) (since C++20)
constexpr std::chrono::year_month&
    operator-=( const std::chrono::years& dy ) const noexcept;  // (3) (since C++20)
constexpr std::chrono::year_month&
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
    auto ym{std::chrono::day(1)/7/2023};

    ym -= std::chrono::years{2};
    assert(ym.month() == std::chrono::July);
    assert(ym.year() == std::chrono::year(2021));

    ym += std::chrono::months{7};
    assert(ym.month() == std::chrono::month(2));
    assert(ym.year() == std::chrono::year(2022));
}
```

### See also

- **operator+operator- (C++20)** — performs arithmetic on `year_month`
  (function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/year_month/operator_arith*
