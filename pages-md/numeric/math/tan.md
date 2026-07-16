# std::tan, std::tanf, std::tanl

```cpp
float       tan ( float num );
double      tan ( double num );
long double tan ( long double num );  // (until C++23)
/* floating-point-type */
            tan ( /* floating-point-type */ num );  // (since C++23) (constexpr since C++26)
float       tanf( float num );  // (2) (since C++11) (constexpr since C++26)
long double tanl( long double num );  // (3) (since C++11) (constexpr since C++26)
Additional overloads (since C++11)
template< class Integer >
double      tan ( Integer num );  // (A) (constexpr since C++26)
```

1-3) Computes the tangent of `num` (measured in radians). The library provides
   overloads of `std::tan` for all cv-unqualified floating-point types as the
   type of the parameter.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
double.
*(since C++11)*

### Parameters

- **num** — floating-point or integer value representing angle in radians

### Return value

If no errors occur, the tangent of `num` (tan(num)) is returned.

The result may have little or no significance if the magnitude of `num` is
large.
*(until C++11)*

If a domain error occurs, an implementation-defined value is returned (NaN where
supported).

If a range error occurs due to underflow, the correct result (after rounding) is
returned.

### Error handling

Errors are reported as specified in `math_errhandling`.

If the implementation supports IEEE floating-point arithmetic (IEC 60559),

- if the argument is ±0, it is returned unmodified.
- if the argument is ±∞, NaN is returned and `FE_INVALID` is raised.
- if the argument is NaN, NaN is returned.

### Notes

The case where the argument is infinite is not specified to be a domain error in
C (to which C++ defers), but it is defined as a domain error in POSIX.

The function has mathematical poles at π(1/2 + n); however no common
floating-point representation is able to represent π/2 exactly, thus there is no
value of the argument for which a pole error occurs.

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their argument `num` of integer
type, `std::tan(num)` has the same effect as
`std::tan(static_cast<double>(num))`.

### Example

```cpp
#include <cerrno>
#include <cfenv>
#include <cmath>
#include <iostream>

// #pragma STDC FENV_ACCESS ON
const double pi = std::acos(-1); // or C++20's std::numbers::pi

int main()
{
    // typical usage
    std::cout << "tan(1*pi/4) = " << std::tan(1*pi/4) << '\n' // 45°
              << "tan(3*pi/4) = " << std::tan(3*pi/4) << '\n' // 135°
              << "tan(5*pi/4) = " << std::tan(5*pi/4) << '\n' // -135°
              << "tan(7*pi/4) = " << std::tan(7*pi/4) << '\n'; // -45°

    // special values
    std::cout << "tan(+0) = " << std::tan(0.0) << '\n'
              << "tan(-0) = " << std::tan(-0.0) << '\n';

    // error handling
    std::feclearexcept(FE_ALL_EXCEPT);

    std::cout << "tan(INFINITY) = " << std::tan(INFINITY) << '\n';
    if (std::fetestexcept(FE_INVALID))
        std::cout << "    FE_INVALID raised\n";
}
```

Possible output:

```text
tan(1*pi/4) = 1
tan(3*pi/4) = -1
tan(5*pi/4) = 1
tan(7*pi/4) = -1
tan(+0) = 0
tan(-0) = -0
tan(INFINITY) = -nan
    FE_INVALID raised
```

### See also

- **sinsinfsinl (C++11)(C++11)** — computes sine (\({\small\sin{x}}\)sin(x))
  (function)
- **coscosfcosl (C++11)(C++11)** — computes cosine (\({\small\cos{x}}\)cos(x))
  (function)
- **atanatanfatanl (C++11)(C++11)** — computes arc tangent
  (\({\small\arctan{x}}\)arctan(x)) (function)
- **tan(std::complex)** — computes tangent of a complex number
  (\({\small\tan{z}}\)tan(z)) (function template)
- **tan(std::valarray)** — applies the function `std::tan` to each element of
  valarray (function template)

**C documentation for `tan`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/tan*
