# std::sqrt, std::sqrtf, std::sqrtl

```cpp
float       sqrt ( float num );
double      sqrt ( double num );
long double sqrt ( long double num );  // (until C++23)
/* floating-point-type */
            sqrt ( /* floating-point-type */ num );  // (since C++23) (constexpr since C++26)
float       sqrtf( float num );  // (2) (since C++11) (constexpr since C++26)
long double sqrtl( long double num );  // (3) (since C++11) (constexpr since C++26)
Additional overloads (since C++11)
template< class Integer >
double      sqrt ( Integer num );  // (A) (constexpr since C++26)
```

1-3) Computes the square root of `num`. The library provides overloads of
   `std::sqrt` for all cv-unqualified floating-point types as the type of the
   parameter.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
double.
*(since C++11)*

### Parameters

- **num** — floating-point or integer value

### Return value

If no errors occur, square root of `num` (\({\small \sqrt{num} }\)√num), is
returned.

If a domain error occurs, an implementation-defined value is returned (NaN where
supported).

If a range error occurs due to underflow, the correct result (after rounding) is
returned.

### Error handling

Errors are reported as specified in `math_errhandling`.

Domain error occurs if `num` is less than zero.

If the implementation supports IEEE floating-point arithmetic (IEC 60559),

- If the argument is less than -0, `FE_INVALID` is raised and NaN is returned.
- If the argument is +∞ or ±0, it is returned, unmodified.
- If the argument is NaN, NaN is returned.

### Notes

`std::sqrt` is required by the IEEE standard to be correctly rounded from the
infinitely precise result. In particular, the exact result is produced if it can
be represented in the floating-point type. The only other operations which
require this are the arithmetic operators and the function `std::fma`. Other
functions, including `std::pow`, are not so constrained.

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their argument `num` of integer
type, `std::sqrt(num)` has the same effect as
`std::sqrt(static_cast<double>(num))`.

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
    // normal use
    std::cout << "sqrt(100) = " << std::sqrt(100) << '\n'
              << "sqrt(2) = " << std::sqrt(2) << '\n'
              << "golden ratio = " << (1 + std::sqrt(5)) / 2 << '\n';

    // special values
    std::cout << "sqrt(-0) = " << std::sqrt(-0.0) << '\n';

    // error handling
    errno = 0;
    std::feclearexcept(FE_ALL_EXCEPT);

    std::cout << "sqrt(-1.0) = " << std::sqrt(-1) << '\n';
    if (errno == EDOM)
        std::cout << "    errno = EDOM " << std::strerror(errno) << '\n';
    if (std::fetestexcept(FE_INVALID))
        std::cout << "    FE_INVALID raised\n";
}
```

Possible output:

```text
sqrt(100) = 10
sqrt(2) = 1.41421
golden ratio = 1.61803
sqrt(-0) = -0
sqrt(-1.0) = -nan
    errno = EDOM Numerical argument out of domain
    FE_INVALID raised
```

### See also

- **powpowfpowl (C++11)(C++11)** — raises a number to the given power
  (\(\small{x^y}\)xy) (function)
- **cbrtcbrtfcbrtl (C++11)(C++11)(C++11)** — computes cube root
  (\(\small{\sqrt[3]{x}}\)3√x) (function)
- **hypothypotfhypotl (C++11)(C++11)(C++11)** — computes square root of the sum
  of the squares of two or three(since C++17) given numbers
  (\(\scriptsize{\sqrt{x^2+y^2}}\)√x2+y2),
  (\(\scriptsize{\sqrt{x^2+y^2+z^2}}\)√x2+y2+z2)(since C++17) (function)
- **sqrt(std::complex)** — complex square root in the range of the right
  half-plane (function template)
- **sqrt(std::valarray)** — applies the function `std::sqrt` to each element of
  valarray (function template)

**C documentation for `sqrt`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/sqrt*
