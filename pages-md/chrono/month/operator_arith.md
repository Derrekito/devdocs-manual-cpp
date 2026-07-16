# std::chrono::month::operator+=, std::chrono::month::operator-=

```cpp
constexpr std::chrono::month& operator+=( const std::chrono::months& m ) noexcept;  // (1) (since C++20)
constexpr std::chrono::month& operator-=( const std::chrono::months& m ) noexcept;  // (2) (since C++20)
```

Adds or subtracts `m.count()` from the month value, reducing the result modulo
12 to an integer in the range `[``1``,``12``]`.

1) Performs `*this = *this + m;`.

2) Performs `*this = *this - m;`.

### Return value

A reference to this `month` after modification.

### Notes

After a call to one of these functions, `ok()` is always `true` if no overflow
occurred during the operation.

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    std::cout << std::boolalpha;

    std::chrono::month m{6};
    m += std::chrono::months(2);
    std::cout << (m == std::chrono::month(8)) << ' '
              << (m == std::chrono::August) << ' ';

    m -= std::chrono::months(3);
    std::cout << (m == std::chrono::month(5)) << ' '
              << (m == std::chrono::May) << ' ';

    m = std::chrono::October;
    m += std::chrono::months{8}; // ((10 += 8 == 18) % 12) == 6;
    std::cout << (m == std::chrono::June) << ' ';

    m -= std::chrono::months{std::chrono::December - std::chrono::February}; // -= 10
    // (6 -= 10) == -4; -4 % 12 == (12 - 4) == 8
    std::cout << (m == std::chrono::August) << '\n';
}
```

Output:

```text
true true true true true true
```

### See also

- **operator++operator++(int)operator--operator--(int)** — increments or
  decrements the month (public member function)
- **operator+operator- (C++20)** — performs arithmetic on `month`s (function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/month/operator_arith*
