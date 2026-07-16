# std::asinh, std::asinhf, std::asinhl

```cpp
float       asinh ( float num );
double      asinh ( double num );
long double asinh ( long double num );  // (until C++23)
/* floating-point-type */
            asinh ( /* floating-point-type */ num );  // (since C++23) (constexpr since C++26)
float       asinhf( float num );  // (2) (since C++11) (constexpr since C++26)
long double asinhl( long double num );  // (3) (since C++11) (constexpr since C++26)
Additional overloads (since C++11)
template< class Integer >
double      asinh ( Integer num );  // (A) (constexpr since C++26)
```

1-3) Computes the inverse hyperbolic sine of `num`. The library provides
   overloads of `std::asinh` for all cv-unqualified floating-point types as the
   type of the parameter.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
double.
*(since C++11)*

### Parameters

- **num** — floating-point or integer value

### Return value

If no errors occur, the inverse hyperbolic sine of `num` (sinh-1(num), or
arsinh(num)), is returned.

If a range error occurs due to underflow, the correct result (after rounding) is
returned.

### Error handling

Errors are reported as specified in `math_errhandling`.

If the implementation supports IEEE floating-point arithmetic (IEC 60559),

- if the argument is ±0 or ±∞, it is returned unmodified.
- if the argument is NaN, NaN is returned.

### Notes

Although the C standard (to which C++ refers for this function) names this
function "arc hyperbolic sine", the inverse functions of the hyperbolic
functions are the area functions. Their argument is the area of a hyperbolic
sector, not an arc. The correct name is "inverse hyperbolic sine" (used by
POSIX) or "area hyperbolic sine".

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their argument `num` of integer
type, `std::asinh(num)` has the same effect as
`std::asinh(static_cast<double>(num))`.

### Examples

```cpp
#include <cmath>
#include <iostream>

int main()
{
    std::cout << "asinh(1) = " << std::asinh(1) << '\n'
              << "asinh(-1) = " << std::asinh(-1) << '\n';

    // special values
    std::cout << "asinh(+0) = " << std::asinh(+0.0) << '\n'
              << "asinh(-0) = " <<  std::asinh(-0.0) << '\n';
}
```

Output:

```text
asinh(1) = 0.881374
asinh(-1) = -0.881374
asinh(+0) = 0
asinh(-0) = -0
```

### See also

- **acoshacoshfacoshl (C++11)(C++11)(C++11)** — computes the inverse hyperbolic
  cosine (\({\small\operatorname{arcosh}{x}}\)arcosh(x)) (function)
- **atanhatanhfatanhl (C++11)(C++11)(C++11)** — computes the inverse hyperbolic
  tangent (\({\small\operatorname{artanh}{x}}\)artanh(x)) (function)
- **sinhsinhfsinhl (C++11)(C++11)** — computes hyperbolic sine
  (\({\small\sinh{x}}\)sinh(x)) (function)
- **asinh(std::complex) (C++11)** — computes area hyperbolic sine of a complex
  number (\({\small\operatorname{arsinh}{z}}\)arsinh(z)) (function template)

**C documentation for `asinh`**

### External links

  Weisstein, Eric W. "Inverse Hyperbolic Sine." From MathWorld — A Wolfram Web
      Resource.

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/asinh*
