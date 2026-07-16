# std::atanh, std::atanhf, std::atanhl

```cpp
float       atanh ( float num );
double      atanh ( double num );
long double atanh ( long double num );  // (until C++23)
/* floating-point-type */
            atanh ( /* floating-point-type */ num );  // (since C++23) (constexpr since C++26)
float       atanhf( float num );  // (2) (since C++11) (constexpr since C++26)
long double atanhl( long double num );  // (3) (since C++11) (constexpr since C++26)
Additional overloads (since C++11)
template< class Integer >
double      atanh ( Integer num );  // (A) (constexpr since C++26)
```

1-3) Computes the inverse hyperbolic tangent of `num`. The library provides
   overloads of `std::atanh` for all cv-unqualified floating-point types as the
   type of the parameter.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
double.
*(since C++11)*

### Parameters

- **num** — floating-point or integer value

### Return value

If no errors occur, the inverse hyperbolic tangent of `num` (tanh-1(num), or
artanh(num)), is returned.

If a domain error occurs, an implementation-defined value is returned (NaN where
supported).

If a pole error occurs, `±HUGE_VAL`, `±HUGE_VALF`, or `±HUGE_VALL` is returned
(with the correct sign).

If a range error occurs due to underflow, the correct result (after rounding) is
returned.

### Error handling

Errors are reported as specified in `math_errhandling`.

If the argument is not on the interval `[``-1``,``+1``]`, a range error occurs.

If the argument is ±1, a pole error occurs.

If the implementation supports IEEE floating-point arithmetic (IEC 60559),

- if the argument is ±0, it is returned unmodified.
- if the argument is ±1, ±∞ is returned and `FE_DIVBYZERO` is raised.
- if |num|>1, NaN is returned and `FE_INVALID` is raised.
- if the argument is NaN, NaN is returned.

### Notes

Although the C standard (to which C++ refers for this function) names this
function "arc hyperbolic tangent", the inverse functions of the hyperbolic
functions are the area functions. Their argument is the area of a hyperbolic
sector, not an arc. The correct name is "inverse hyperbolic tangent" (used by
POSIX) or "area hyperbolic tangent".

POSIX specifies that in case of underflow, `num` is returned unmodified, and if
that is not supported, an implementation-defined value no greater than
`DBL_MIN`, `FLT_MIN`, and `LDBL_MIN` is returned.

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their argument `num` of integer
type, `std::atanh(num)` has the same effect as
`std::atanh(static_cast<double>(num))`.

### Example

```cpp
#include <cerrno>
#include <cfenv>
#include <cfloat>
#include <cmath>
#include <cstring>
#include <iostream>
// #pragma STDC FENV_ACCESS ON

int main()
{
    std::cout << "atanh(0) = " << std::atanh(0) << '\n'
              << "atanh(-0) = " << std::atanh(-0.0) << '\n'
              << "atanh(0.9) = " << std::atanh(0.9) << '\n';

    // error handling
    errno = 0;
    std::feclearexcept(FE_ALL_EXCEPT);

    std::cout << "atanh(-1) = " << std::atanh(-1) << '\n';

    if (errno == ERANGE)
        std::cout << "    errno == ERANGE: " << std::strerror(errno) << '\n';
    if (std::fetestexcept(FE_DIVBYZERO))
        std::cout << "    FE_DIVBYZERO raised\n";
}
```

Possible output:

```text
atanh(0) = 0
atanh(-0) = -0
atanh(0.9) = 1.47222
atanh(-1) = -inf
    errno == ERANGE: Numerical result out of range
    FE_DIVBYZERO raised
```

### See also

- **asinhasinhfasinhl (C++11)(C++11)(C++11)** — computes the inverse hyperbolic
  sine (\({\small\operatorname{arsinh}{x}}\)arsinh(x)) (function)
- **acoshacoshfacoshl (C++11)(C++11)(C++11)** — computes the inverse hyperbolic
  cosine (\({\small\operatorname{arcosh}{x}}\)arcosh(x)) (function)
- **tanhtanhftanhl (C++11)(C++11)** — computes hyperbolic tangent
  (\({\small\tanh{x}}\)tanh(x)) (function)
- **atanh(std::complex) (C++11)** — computes area hyperbolic tangent of a
  complex number (\({\small\operatorname{artanh}{z}}\)artanh(z)) (function
  template)

**C documentation for `atanh`**

### External links

  Weisstein, Eric W. "Inverse Hyperbolic Tangent." From MathWorld — A Wolfram
      Web Resource.

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/atanh*
