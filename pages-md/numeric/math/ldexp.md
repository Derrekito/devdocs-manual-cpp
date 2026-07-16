# std::ldexp, std::ldexpf, std::ldexpl

```cpp
float       ldexp ( float num, int exp );
double      ldexp ( double num, int exp );
long double ldexp ( long double num, int exp );  // (until C++23)
constexpr /* floating-point-type */
            ldexp ( /* floating-point-type */ num, int exp );  // (since C++23)
float       ldexpf( float num, int exp );  // (2) (since C++11) (constexpr since C++23)
long double ldexpl( long double num, int exp );  // (3) (since C++11) (constexpr since C++23)
Additional overloads (since C++11)
template< class Integer >
double      ldexp ( Integer num, int exp );  // (A) (since C++11) (constexpr since C++23)
```

1-3) Multiplies a floating point value `num` by the number `2` raised to the
   `exp` power. The library provides overloads of `std::ldexp` for all
   cv-unqualified floating-point types as the type of the parameter `num`.(since
   C++23)

A) Additional overloads are provided for all integer types, which are treated as
double.
*(since C++11)*

### Parameters

- **num** — floating-point or integer value
- **exp** — integer value

### Return value

If no errors occur, `num` multiplied by 2 to the power of `exp` (num×2exp) is
returned.

If a range error due to overflow occurs, `±HUGE_VAL`, `±HUGE_VALF`, or
`±HUGE_VALL` is returned.

If a range error due to underflow occurs, the correct result (after rounding) is
returned.

### Error handling

Errors are reported as specified in `math_errhandling`.

If the implementation supports IEEE floating-point arithmetic (IEC 60559),

- Unless a range error occurs, `FE_INEXACT` is never raised (the result is
  exact).
- Unless a range error occurs, the current rounding mode is ignored.
- If `num` is ±0, it is returned, unmodified.
- If `num` is ±∞, it is returned, unmodified.
- If `exp` is 0, then `num` is returned, unmodified.
- If `num` is NaN, NaN is returned.

### Notes

On binary systems (where `FLT_RADIX` is `2`), `std::ldexp` is equivalent to
`std::scalbn`.

The function `std::ldexp` ("load exponent"), together with its dual,
`std::frexp`, can be used to manipulate the representation of a floating-point
number without direct bit manipulations.

On many implementations, `std::ldexp` is less efficient than multiplication or
division by a power of two using arithmetic operators.

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their argument `num` of integer
type, `std::ldexp(num, exp)` has the same effect as
`std::ldexp(static_cast<double>(num), exp)`.

### Example

```cpp
#include <cerrno>
#include <cfenv>
#include <cmath>
#include <cstring>
#include <iostream>
// #pragma STDC FENV_ACCESS ON

int main()
{
    std::cout
        << "ldexp(5, 3) = 5 * 8 = " << std::ldexp(5, 3) << '\n'
        << "ldexp(7, -4) = 7 / 16 = " << std::ldexp(7, -4) << '\n'
        << "ldexp(1, -1074) = " << std::ldexp(1, -1074)
        << " (minimum positive subnormal float64_t)\n"
        << "ldexp(nextafter(1,0), 1024) = "
        << std::ldexp(std::nextafter(1,0), 1024)
        << " (largest finite float64_t)\n";

    // special values
    std::cout << "ldexp(-0, 10) = " << std::ldexp(-0.0, 10) << '\n'
              << "ldexp(-Inf, -1) = " << std::ldexp(-INFINITY, -1) << '\n';

    // error handling
    std::feclearexcept(FE_ALL_EXCEPT);
    errno = 0;
    const double inf = std::ldexp(1, 1024);
    const bool is_range_error = errno == ERANGE;

    std::cout << "ldexp(1, 1024) = " << inf << '\n';
    if (is_range_error)
        std::cout << "    errno == ERANGE: " << std::strerror(ERANGE) << '\n';
    if (std::fetestexcept(FE_OVERFLOW))
        std::cout << "    FE_OVERFLOW raised\n";
}
```

Possible output:

```text
ldexp(5, 3) = 5 * 8 = 40
ldexp(7, -4) = 7 / 16 = 0.4375
ldexp(1, -1074) = 4.94066e-324 (minimum positive subnormal float64_t)
ldexp(nextafter(1,0), 1024) = 1.79769e+308 (largest finite float64_t)
ldexp(-0, 10) = -0
ldexp(-Inf, -1) = -inf
ldexp(1, 1024) = inf
    errno == ERANGE: Numerical result out of range
    FE_OVERFLOW raised
```

### See also

- **frexpfrexpffrexpl (C++11)(C++11)** — decomposes a number into significand
  and a power of `2` (function)
- **scalbnscalbnfscalbnlscalblnscalblnfscalblnl
  (C++11)(C++11)(C++11)(C++11)(C++11)(C++11)** — multiplies a number by
  `FLT_RADIX` raised to a power (function)

**C documentation for `ldexp`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/ldexp*
