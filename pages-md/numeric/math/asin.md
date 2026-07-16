# std::asin, std::asinf, std::asinl

```cpp
float       asin ( float num );
double      asin ( double num );
long double asin ( long double num );  // (until C++23)
/* floating-point-type */
            asin ( /* floating-point-type */ num );  // (since C++23) (constexpr since C++26)
float       asinf( float num );  // (2) (since C++11) (constexpr since C++26)
long double asinl( long double num );  // (3) (since C++11) (constexpr since C++26)
Additional overloads (since C++11)
template< class Integer >
double      asin ( Integer num );  // (A) (constexpr since C++26)
```

1-3) Computes the principal value of the arc sine of `num`. The library provides
   overloads of `std::asin` for all cv-unqualified floating-point types as the
   type of the parameter.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
double.
*(since C++11)*

### Parameters

- **num** — floating-point or integer value

### Return value

If no errors occur, the arc sine of `num` (arcsin(num)) in the range [- π/2 ,
+π/2], is returned.

If a domain error occurs, an implementation-defined value is returned (NaN where
supported).

If a range error occurs due to underflow, the correct result (after rounding) is
returned.

### Error handling

Errors are reported as specified in `math_errhandling`.

Domain error occurs if `num` is outside the range `[``-1.0``,``1.0``]`.

If the implementation supports IEEE floating-point arithmetic (IEC 60559),

- If the argument is ±0, it is returned unmodified.
- If |num| > 1, a domain error occurs and NaN is returned.
- if the argument is NaN, NaN is returned.

### Notes

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their argument `num` of integer
type, `std::asin(num)` has the same effect as
`std::asin(static_cast<double>(num))`.

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
    std::cout << "asin(1.0) = " << asin(1) << '\n'
              << "2*asin(1.0) = " << 2 * asin(1) << '\n'
              << "asin(-0.5) = " << asin(-0.5) << '\n'
              << "6*asin(-0.5) =" << 6 * asin(-0.5) << '\n';

    // special values
    std::cout << "asin(0.0) = " << asin(0) << " asin(-0.0)=" << asin(-0.0) << '\n';

    // error handling
    errno = 0;
    std::feclearexcept(FE_ALL_EXCEPT);

    std::cout << "asin(1.1) = " << asin(1.1) << '\n';

    if (errno == EDOM)
        std::cout << "    errno == EDOM: " << std::strerror(errno) << '\n';
    if (std::fetestexcept(FE_INVALID))
        std::cout << "    FE_INVALID raised" << '\n';
}
```

Possible output:

```text
asin(1.0) = 1.5708
2*asin(1.0) = 3.14159
asin(-0.5) = -0.523599
6*asin(-0.5) = -3.14159
asin(0.0) = 0 asin(-0.0)=-0
asin(1.1) = nan
    errno == EDOM: Numerical argument out of domain
    FE_INVALID raised
```

### See also

- **acosacosfacosl (C++11)(C++11)** — computes arc cosine
  (\({\small\arccos{x}}\)arccos(x)) (function)
- **atanatanfatanl (C++11)(C++11)** — computes arc tangent
  (\({\small\arctan{x}}\)arctan(x)) (function)
- **atan2atan2fatan2l (C++11)(C++11)** — arc tangent, using signs to determine
  quadrants (function)
- **sinsinfsinl (C++11)(C++11)** — computes sine (\({\small\sin{x}}\)sin(x))
  (function)
- **asin(std::complex) (C++11)** — computes arc sine of a complex number
  (\({\small\arcsin{z}}\)arcsin(z)) (function template)
- **asin(std::valarray)** — applies the function `std::asin` to each element of
  valarray (function template)

**C documentation for `asin`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/asin*
