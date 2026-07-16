# std::abs(float), std::fabs, std::fabsf, std::fabsl

```cpp
float       abs( float num );
double      abs( double num );
long double abs( long double num );  // (until C++23)
constexpr /* floating-point-type */
            abs( /* floating-point-type */ num );  // (since C++23)
float       fabs ( float num );
double      fabs ( double num );
long double fabs ( long double num );  // (until C++23)
constexpr /* floating-point-type */
            fabs ( /* floating-point-type */ num );  // (since C++23)
float       fabsf( float num );  // (3) (since C++11) (constexpr since C++23)
long double fabsl( long double num );  // (4) (since C++11) (constexpr since C++23)
Additional overloads (since C++11)
template< class Integer >
double      fabs ( Integer num );  // (A) (since C++11) (constexpr since C++23)
```

1-4) Computes the absolute value of the floating-point value `num`. The library
   provides overloads of `std::abs` and `std::fabs` for all cv-unqualified
   floating-point types as the type of the parameter `num`.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
double.
*(since C++11)*

For integral arguments, the integral overloads of `std::abs` are likely better
matches. If `std::abs` is called with an unsigned integral argument that cannot
be converted to int by integral promotion, the program is ill-formed.

### Parameters

- **num** — floating-point or integer value

### Return value

If successful, returns the absolute value of `arg` (`|arg|`). The value returned
is exact and does not depend on any rounding modes.

### Error handling

This function is not subject to any of the error conditions specified in
`math_errhandling`.

If the implementation supports IEEE floating-point arithmetic (IEC 60559),

- If the argument is ±0, +0 is returned.
- If the argument is ±∞, +∞ is returned.
- If the argument is NaN, NaN is returned.

### Notes

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their argument `num` of integer
type, `std::fabs(num)` has the same effect as
`std::fabs(static_cast<double>(num))`.

### Example

```cpp
#include <cmath>
#include <iostream>

int main()
{
    std::cout << "abs(+3.0) = " << std::abs(+3.0) << '\n'
              << "abs(-3.0) = " << std::abs(-3.0) << '\n';

    // special values
    std::cout << "abs(-0.0) = " << std::abs(-0.0) << '\n'
              << "abs(-Inf) = " << std::abs(-INFINITY) << '\n'
              << "abs(-NaN) = " << std::abs(-NAN) << '\n';
}
```

Possible output:

```text
abs(+3.0) = 3
abs(-3.0) = 3
abs(-0.0) = 0
abs(-Inf) = inf
abs(-NaN) = nan
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2192 | C++98 | overloads of `std::abs` were inconsistently declared in two
      headers | declared these overloads in both headers
  LWG 2735 | C++11 | overloads of `std::abs` for integer types returning double
      was erroneously required | removed the requirement

### See also

- **abs(int)labsllabs (C++11)** — computes absolute value of an integral value
  (\(\small{|x|}\)|x|) (function)
- **copysigncopysignfcopysignl (C++11)(C++11)(C++11)** — copies the sign of a
  floating point value (function)
- **signbit (C++11)** — checks if the given number is negative (function)
- **abs(std::complex)** — returns the magnitude of a complex number (function
  template)
- **abs(std::valarray)** — applies the function `abs` to each element of
  valarray (function template)

**C documentation for `fabs`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/fabs*
