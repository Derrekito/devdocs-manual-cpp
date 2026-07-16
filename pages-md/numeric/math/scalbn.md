# std::scalbn, std::scalbnf, std::scalbnl, std::scalbln, std::scalblnf, std::scalblnl

```cpp
int
float       scalbn ( float num, int exp );
double      scalbn ( double num, int exp );
long double scalbn ( long double num, int exp );  // (since C++11) (until C++23)
constexpr /* floating-point-type */
            scalbn ( /* floating-point-type */ num, int exp );  // (since C++23)
float       scalbnf( float num, int exp );  // (2) (since C++11) (constexpr since C++23)
long double scalbnl( long double num, int exp );  // (3) (since C++11) (constexpr since C++23)
long
float       scalbln ( float num, long exp );
double      scalbln ( double num, long exp );
long double scalbln ( long double num, long exp );  // (since C++11) (until C++23)
constexpr /* floating-point-type */
            scalbln ( /* floating-point-type */ num, long exp );  // (since C++23)
float       scalblnf( float num, long exp );  // (5) (since C++11) (constexpr since C++23)
long double scalblnl( long double num, long exp );  // (6) (since C++11) (constexpr since C++23)
Additional overloads
template< class Integer >
double scalbn( Integer num, int exp );  // (A) (since C++11) (constexpr since C++23)
template< class Integer >
double scalbln( Integer num, long exp );  // (B) (since C++11) (constexpr since C++23)
```

1-6) Multiplies a floating point value `num` by `FLT_RADIX` raised to power
   `exp`. The library provides overloads of `std::scalbn` and `std::scalbln` for
   all cv-unqualified floating-point types as the type of the parameter
   `num`.(since C++23)

A,B) Additional overloads are provided for all integer types, which are treated
   as `double`.

### Parameters

- **num** — floating-point or integer value
- **exp** — integer value

### Return value

If no errors occur, `num` multiplied by `FLT_RADIX` to the power of `exp`
(num×FLT_RADIXexp) is returned.

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

On binary systems (where `FLT_RADIX` is `2`), `std::scalbn` is equivalent to
`std::ldexp`.

Although `std::scalbn` and `std::scalbln` are specified to perform the operation
efficiently, on many implementations they are less efficient than multiplication
or division by a power of two using arithmetic operators.

The function name stands for "new scalb", where `scalb` was an older
non-standard function whose second argument had floating-point type.

The `std::scalbln` function is provided because the factor required to scale
from the smallest positive floating-point value to the largest finite one may be
greater than 32767, the standard-guaranteed `INT_MAX`. In particular, for the
80-bit long double, the factor is 32828.

The GNU implementation does not set `errno` regardless of `math_errhandling`.

The additional overloads are not required to be provided exactly as (A,B). They
only need to be sufficient to ensure that for their argument `num` of integer
type:

- `std::scalbn(num, exp)` has the same effect as
  `std::scalbn(static_cast<double>(num), exp)`.
- `std::scalbln(num, exp)` has the same effect as
  `std::scalbln(static_cast<double>(num), exp)`.

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
    std::cout << "scalbn(7, -4) = " << std::scalbn(7, -4) << '\n'
              << "scalbn(1, -1074) = " << std::scalbn(1, -1074)
              << " (minimum positive subnormal double)\n"
              << "scalbn(nextafter(1,0), 1024) = "
              << std::scalbn(std::nextafter(1,0), 1024)
              << " (largest finite double)\n";

    // special values
    std::cout << "scalbn(-0, 10) = " << std::scalbn(-0.0, 10) << '\n'
              << "scalbn(-Inf, -1) = " << std::scalbn(-INFINITY, -1) << '\n';

    // error handling
    errno = 0;
    std::feclearexcept(FE_ALL_EXCEPT);

    std::cout << "scalbn(1, 1024) = " << std::scalbn(1, 1024) << '\n';

    if (errno == ERANGE)
        std::cout << "    errno == ERANGE: " << std::strerror(errno) << '\n';
    if (std::fetestexcept(FE_OVERFLOW))
        std::cout << "    FE_OVERFLOW raised\n";
}
```

Possible output:

```text
scalbn(7, -4) = 0.4375
scalbn(1, -1074) = 4.94066e-324 (minimum positive subnormal double)
scalbn(nextafter(1,0), 1024) = 1.79769e+308 (largest finite double)
scalbn(-0, 10) = -0
scalbn(-Inf, -1) = -inf
scalbn(1, 1024) = inf
    errno == ERANGE: Numerical result out of range
    FE_OVERFLOW raised
```

### See also

- **frexpfrexpffrexpl (C++11)(C++11)** — decomposes a number into significand
  and a power of `2` (function)
- **ldexpldexpfldexpl (C++11)(C++11)** — multiplies a number by `2` raised to a
  power (function)

**C documentation for `scalbn`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/scalbn*
