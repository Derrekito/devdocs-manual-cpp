# std::cbrt, std::cbrtf, std::cbrtl

```cpp
float       cbrt ( float num );
double      cbrt ( double num );
long double cbrt ( long double num );  // (until C++23)
/* floating-point-type */
            cbrt ( /* floating-point-type */ num );  // (since C++23) (constexpr since C++26)
float       cbrtf( float num );  // (2) (since C++11) (constexpr since C++26)
long double cbrtl( long double num );  // (3) (since C++11) (constexpr since C++26)
Additional overloads (since C++11)
template< class Integer >
double      cbrt ( Integer num );  // (A) (constexpr since C++26)
```

1-3) Computes the cube root of `num`. The library provides overloads of
   `std::cbrt` for all cv-unqualified floating-point types as the type of the
   parameter.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
double.
*(since C++11)*

### Parameters

- **num** — floating-point or integer value

### Return value

If no errors occur, the cube root of `num` (\(\small{\sqrt[3]{num} }\)3√num), is
returned.

If a range error occurs due to underflow, the correct result (after rounding) is
returned.

### Error handling

Errors are reported as specified in `math_errhandling`.

If the implementation supports IEEE floating-point arithmetic (IEC 60559),

- if the argument is ±0 or ±∞, it is returned, unchanged.
- if the argument is NaN, NaN is returned.

### Notes

`std::cbrt(num)` is not equivalent to `std::pow(num, 1.0 / 3)` because the
rational number \(\small{\frac1{3} }\)1/3 is typically not equal to `1.0 / 3`
and `std::pow` cannot raise a negative base to a fractional exponent. Moreover,
`std::cbrt(num)` usually gives more accurate results than `std::pow(num, 1.0 /
3)` (see example).

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their argument `num` of integer
type, `std::cbrt(num)` has the same effect as
`std::cbrt(static_cast<double>(num))`.

### Example

```cpp
#include <cmath>
#include <iomanip>
#include <iostream>
#include <limits>

int main()
{
    std::cout
        << "Normal use:\n"
        << "cbrt(729)       = " << std::cbrt(729) << '\n'
        << "cbrt(-0.125)    = " << std::cbrt(-0.125) << '\n'
        << "Special values:\n"
        << "cbrt(-0)        = " << std::cbrt(-0.0) << '\n'
        << "cbrt(+inf)      = " << std::cbrt(INFINITY) << '\n'
        << "Accuracy and comparison with `pow`:\n"
        << std::setprecision(std::numeric_limits<double>::max_digits10)
        << "cbrt(343)       = " << std::cbrt(343) << '\n'
        << "pow(343,1.0/3)  = " << std::pow(343, 1.0 / 3) << '\n'
        << "cbrt(-343)      = " << std::cbrt(-343) << '\n'
        << "pow(-343,1.0/3) = " << std::pow(-343, 1.0 / 3) << '\n';
}
```

Possible output:

```text
Normal use:
cbrt(729)       = 9
cbrt(-0.125)    = -0.5
Special values:
cbrt(-0)        = -0
cbrt(+inf)      = inf
Accuracy and comparison with `pow`:
cbrt(343)       = 7
pow(343,1.0/3)  = 6.9999999999999991
cbrt(-343)      = -7
pow(-343,1.0/3) = -nan
```

### See also

- **powpowfpowl (C++11)(C++11)** — raises a number to the given power
  (\(\small{x^y}\)xy) (function)
- **sqrtsqrtfsqrtl (C++11)(C++11)** — computes square root
  (\(\small{\sqrt{x}}\)√x) (function)
- **hypothypotfhypotl (C++11)(C++11)(C++11)** — computes square root of the sum
  of the squares of two or three(since C++17) given numbers
  (\(\scriptsize{\sqrt{x^2+y^2}}\)√x2+y2),
  (\(\scriptsize{\sqrt{x^2+y^2+z^2}}\)√x2+y2+z2)(since C++17) (function)

**C documentation for `cbrt`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/cbrt*
