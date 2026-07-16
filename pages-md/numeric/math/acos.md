# std::acos, std::acosf, std::acosl

```cpp
float       acos ( float num );
double      acos ( double num );
long double acos ( long double num );  // (until C++23)
/* floating-point-type */
            acos ( /* floating-point-type */ num );  // (since C++23) (constexpr since C++26)
float       acosf( float num );  // (2) (since C++11) (constexpr since C++26)
long double acosl( long double num );  // (3) (since C++11) (constexpr since C++26)
Additional overloads (since C++11)
template< class Integer >
double      acos ( Integer num );  // (A) (constexpr since C++26)
```

1-3) Computes the principal value of the arc cosine of `num`. The library
   provides overloads of `std::acos` for all cv-unqualified floating-point types
   as the type of the parameter.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
double.
*(since C++11)*

### Parameters

- **num** — floating-point or integer value

### Return value

If no errors occur, the arc cosine of `num` (arccos(num)) in the range [0, π],
is returned.

If a domain error occurs, an implementation-defined value is returned (NaN where
supported).

If a range error occurs due to underflow, the correct result (after rounding) is
returned.

### Error handling

Errors are reported as specified in `math_errhandling`.

Domain error occurs if `num` is outside the range `[``-1.0``,``1.0``]`.

If the implementation supports IEEE floating-point arithmetic (IEC 60559),

- If the argument is +1, the value `+0` is returned.
- If |num| > 1, a domain error occurs and NaN is returned.
- if the argument is NaN, NaN is returned.

### Notes

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their argument `num` of integer
type, `std::acos(num)` has the same effect as
`std::acos(static_cast<double>(num))`.

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
    std::cout << "acos(-1) = " << std::acos(-1) << '\n'
              << "acos(0.0) = " << std::acos(0.0) << '\n'
              << "2*acos(0.0) = " << 2 * std::acos(0) << '\n'
              << "acos(0.5) = " << std::acos(0.5) << '\n'
              << "3*acos(0.5) = " << 3 * std::acos(0.5) << '\n'
              << "acos(1) = " << std::acos(1) << '\n';

    // error handling
    errno = 0;
    std::feclearexcept(FE_ALL_EXCEPT);

    std::cout << "acos(1.1) = " << std::acos(1.1) << '\n';

    if (errno == EDOM)
        std::cout << "    errno == EDOM: " << std::strerror(errno) << '\n';
    if (std::fetestexcept(FE_INVALID))
        std::cout << "    FE_INVALID raised" << '\n';
}
```

Output:

```text
acos(-1) = 3.14159
acos(0.0) = 1.5708
2*acos(0.0) = 3.14159
acos(0.5) = 1.0472
3*acos(0.5) = 3.14159
acos(1) = 0
acos(1.1) = nan
    errno == EDOM: Numerical argument out of domain
    FE_INVALID raised
```

### See also

- **asinasinfasinl (C++11)(C++11)** — computes arc sine
  (\({\small\arcsin{x}}\)arcsin(x)) (function)
- **atanatanfatanl (C++11)(C++11)** — computes arc tangent
  (\({\small\arctan{x}}\)arctan(x)) (function)
- **atan2atan2fatan2l (C++11)(C++11)** — arc tangent, using signs to determine
  quadrants (function)
- **coscosfcosl (C++11)(C++11)** — computes cosine (\({\small\cos{x}}\)cos(x))
  (function)
- **acos(std::complex) (C++11)** — computes arc cosine of a complex number
  (\({\small\arccos{z}}\)arccos(z)) (function template)
- **acos(std::valarray)** — applies the function `std::acos` to each element of
  valarray (function template)

**C documentation for `acos`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/acos*
