# std::acosh, std::acoshf, std::acoshl

```cpp
float       acosh ( float num );
double      acosh ( double num );
long double acosh ( long double num );  // (until C++23)
/* floating-point-type */
            acosh ( /* floating-point-type */ num );  // (since C++23) (constexpr since C++26)
float       acoshf( float num );  // (2) (since C++11) (constexpr since C++26)
long double acoshl( long double num );  // (3) (since C++11) (constexpr since C++26)
Additional overloads (since C++11)
template< class Integer >
double      acosh ( Integer num );  // (A) (constexpr since C++26)
```

1-3) Computes the inverse hyperbolic cosine of `num`. The library provides
   overloads of `std::acosh` for all cv-unqualified floating-point types as the
   type of the parameter.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
double.
*(since C++11)*

### Parameters

- **num** — floating-point or integer value

### Return value

If no errors occur, the inverse hyperbolic cosine of `num` (cosh-1(num), or
arcosh(num)) on the interval [0, +∞], is returned.

If a domain error occurs, an implementation-defined value is returned (NaN where
supported).

### Error handling

Errors are reported as specified in `math_errhandling`.

If the argument is less than 1, a domain error occurs.

If the implementation supports IEEE floating-point arithmetic (IEC 60559),

- if the argument is less than 1, `FE_INVALID` is raised an NaN is returned.
- if the argument is 1, +0 is returned.
- if the argument is +∞, +∞ is returned.
- if the argument is NaN, NaN is returned.

### Notes

Although the C standard (to which C++ refers for this function) names this
function "arc hyperbolic cosine", the inverse functions of the hyperbolic
functions are the area functions. Their argument is the area of a hyperbolic
sector, not an arc. The correct name is "inverse hyperbolic cosine" (used by
POSIX) or "area hyperbolic cosine".

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their argument `num` of integer
type, `std::acosh(num)` has the same effect as
`std::acosh(static_cast<double>(num))`.

### Examples

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
    std::cout << "acosh(1) = " << std::acosh(1) << '\n'
              << "acosh(10) = " << std::acosh(10) << '\n'
              << "acosh(DBL_MAX) = " << std::acosh(DBL_MAX) << '\n'
              << "acosh(Inf) = " << std::acosh(INFINITY) << '\n';

    // error handling
    errno = 0;
    std::feclearexcept(FE_ALL_EXCEPT);

    std::cout << "acosh(0.5) = " << std::acosh(0.5) << '\n';

    if (errno == EDOM)
        std::cout << "    errno == EDOM: " << std::strerror(errno) << '\n';
    if (std::fetestexcept(FE_INVALID))
        std::cout << "    FE_INVALID raised\n";
}
```

Possible output:

```text
acosh(1) = 0
acosh(10) = 2.99322
acosh(DBL_MAX) = 710.476
acosh(Inf) = inf
acosh(0.5) = -nan
    errno == EDOM: Numerical argument out of domain
    FE_INVALID raised
```

### See also

- **asinhasinhfasinhl (C++11)(C++11)(C++11)** — computes the inverse hyperbolic
  sine (\({\small\operatorname{arsinh}{x}}\)arsinh(x)) (function)
- **atanhatanhfatanhl (C++11)(C++11)(C++11)** — computes the inverse hyperbolic
  tangent (\({\small\operatorname{artanh}{x}}\)artanh(x)) (function)
- **coshcoshfcoshl (C++11)(C++11)** — computes hyperbolic cosine
  (\({\small\cosh{x}}\)cosh(x)) (function)
- **acosh(std::complex) (C++11)** — computes area hyperbolic cosine of a complex
  number (\({\small\operatorname{arcosh}{z}}\)arcosh(z)) (function template)

**C documentation for `acosh`**

### External links

  Weisstein, Eric W. "Inverse Hyperbolic Cosine." From MathWorld — A Wolfram Web
      Resource.

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/acosh*
