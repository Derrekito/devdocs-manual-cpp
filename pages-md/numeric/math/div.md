# std::div, std::ldiv, std::lldiv, std::imaxdiv

```cpp
std::div_t     div( int x, int y );  // (1) (constexpr since C++23)
std::ldiv_t    div( long x, long y );  // (2) (constexpr since C++23)
std::lldiv_t   div( long long x, long long y );  // (3) (since C++11) (constexpr since C++23)
std::ldiv_t   ldiv( long x, long y );  // (4) (constexpr since C++23)
std::lldiv_t lldiv( long long x, long long y );  // (5) (since C++11) (constexpr since C++23)
std::imaxdiv_t div( std::intmax_t x, std::intmax_t y );  // (6) (since C++11) (constexpr since C++23)
std::imaxdiv_t imaxdiv( std::intmax_t x, std::intmax_t y );  // (7) (since C++11) (constexpr since C++23)
```

Computes both the quotient and the remainder of the division of the numerator
`x` by the denominator `y`.

6,7) Overload of `std::div` for `std::intmax_t` is provided in `<cinttypes>` if
and only if `std::intmax_t` is an extended integer type.
*(since C++11)*

The quotient is the algebraic quotient with any fractional part discarded
(truncated towards zero). The remainder is such that `quot * y + rem == x`.
*(until C++11)*

The quotient is the result of the expression `x / y`. The remainder is the
result of the expression `x % y`.
*(since C++11)*

### Parameters

- **x, y** — integer values

### Return value

If both the remainder and the quotient can be represented as objects of the
corresponding type (int, long, long long, `std::intmax_t`, respectively),
returns both as an object of type `std::div_t`, `std::ldiv_t`, `std::lldiv_t`,
`std::imaxdiv_t` defined as follows:

## std::div_t

```cpp
struct div_t { int quot; int rem; };
```

or

```cpp
struct div_t { int rem; int quot; };
```

## std::ldiv_t

```cpp
struct ldiv_t { long quot; long rem; };
```

or

```cpp
struct ldiv_t { long rem; long quot; };
```

## std::lldiv_t

```cpp
struct lldiv_t { long long quot; long long rem; };
```

or

```cpp
struct lldiv_t { long long rem; long long quot; };
```

## std::imaxdiv_t

```cpp
struct imaxdiv_t { std::intmax_t quot; std::intmax_t rem; };
```

or

```cpp
struct imaxdiv_t { std::intmax_t rem; std::intmax_t quot; };
```

If either the remainder or the quotient cannot be represented, the behavior is
undefined.

### Notes

Until C++11, the rounding direction of the quotient and the sign of the
remainder in the built-in division and remainder operators was
implementation-defined if either of the operands was negative, but it was
well-defined in `std::div`.

On many platforms, a single CPU instruction obtains both the quotient and the
remainder, and this function may leverage that, although compilers are generally
able to merge nearby `/` and `%` where suitable.

### Example

```cpp
#include <cassert>
#include <cmath>
#include <cstdlib>
#include <iostream>
#include <sstream>
#include <string>

std::string division_with_remainder_string(int dividend, int divisor)
{
    auto dv = std::div(dividend, divisor);
    assert(dividend == divisor * dv.quot + dv.rem);
    assert(dv.quot == dividend / divisor);
    assert(dv.rem == dividend % divisor);

    auto sign = [](int n){ return n > 0 ? 1 : n < 0 ? -1 : 0; };
    assert((dv.rem == 0) or (sign(dv.rem) == sign(dividend)));

    return (std::ostringstream() << std::showpos << dividend << " = "
                                 << divisor << " * (" << dv.quot << ") "
                                 << std::showpos << dv.rem).str();
}

std::string itoa(int n, int radix /*[2..16]*/)
{
    std::string buf;
    std::div_t dv{}; dv.quot = n;

    do
    {
        dv = std::div(dv.quot, radix);
        buf += "0123456789abcdef"[std::abs(dv.rem)]; // string literals are arrays
    }
    while (dv.quot);

    if (n < 0)
        buf += '-';

    return {buf.rbegin(), buf.rend()};
}

int main()
{
    std::cout << division_with_remainder_string(369, 10) << '\n'
              << division_with_remainder_string(369, -10) << '\n'
              << division_with_remainder_string(-369, 10) << '\n'
              << division_with_remainder_string(-369, -10) << "\n\n";

    std::cout << itoa(12345, 10) << '\n'
              << itoa(-12345, 10) << '\n'
              << itoa(42, 2) << '\n'
              << itoa(65535, 16) << '\n';
}
```

Output:

```text
+369 = +10 * (+36) +9
+369 = -10 * (-36) +9
-369 = +10 * (-36) -9
-369 = -10 * (+36) -9

12345
-12345
101010
ffff
```

### See also

- **fmodfmodffmodl (C++11)(C++11)** — remainder of the floating point division
  operation (function)
- **remainderremainderfremainderl (C++11)(C++11)(C++11)** — signed remainder of
  the division operation (function)
- **remquoremquofremquol (C++11)(C++11)(C++11)** — signed remainder as well as
  the three last bits of the division operation (function)

**C documentation for `div`**

### External links

  1. | Euclidean division — From Wikipedia.
  2. | Modulo (and Truncated division) — From Wikipedia.

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/div*
