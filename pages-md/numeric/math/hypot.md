# std::hypot, std::hypotf, std::hypotl

```cpp
float       hypot ( float x, float y );
double      hypot ( double x, double y );
long double hypot ( long double x, long double y );  // (since C++11) (until C++23)
/* floating-point-type */
            hypot ( /* floating-point-type */ x,
                    /* floating-point-type */ y );  // (since C++23) (constexpr since C++26)
float       hypotf( float x, float y );  // (2) (since C++11) (constexpr since C++26)
long double hypotl( long double x, long double y );  // (3) (since C++11) (constexpr since C++26)
float       hypot ( float x, float y, float z );
double      hypot ( double x, double y, double z );
long double hypot ( long double x, long double y, long double z );  // (since C++17) (until C++23)
/* floating-point-type */
            hypot ( /* floating-point-type */ x,
                    /* floating-point-type */ y,
                    /* floating-point-type */ z );  // (since C++23) (constexpr since C++26)
Additional overloads
template< class Arithmetic1, Arithmetic2 >
/* common-floating-point-type */
            hypot ( Arithmetic1 x, Arithmetic2 y );  // (A) (since C++11) (constexpr since C++26)
template< class Arithmetic1, Arithmetic2, Arithmetic3 >
/* common-floating-point-type */
            hypot ( Arithmetic1 x, Arithmetic2 y, Arithmetic3 z );  // (B) (since C++17) (constexpr since C++26)
```

1-3) Computes the square root of the sum of the squares of `x` and `y`, without
   undue overflow or underflow at intermediate stages of the computation.The
   library provides overloads of `std::hypot` for all cv-unqualified
   floating-point types as the type of the parameters `x` and `y`.(since C++23)

4) Computes the square root of the sum of the squares of `x`, `y`, and `z`,
   without undue overflow or underflow at intermediate stages of the
   computation.The library provides overloads of `std::hypot` for all
   cv-unqualified floating-point types as the type of the parameters `x`, `y`
   and `z`.(since C++23)

A,B) Additional overloads are provided for all other combinations of arithmetic
   types.

The value computed by the two-argument version of this function is the length of
the hypotenuse of a right-angled triangle with sides of length `x` and `y`, or
the distance of the point `(x,y)` from the origin `(0,0)`, or the magnitude of a
complex number `x+iy`.

The value computed by the three-argument version of this function is the
distance of the point `(x,y,z)` from the origin `(0,0,0)`.

### Parameters

- **x, y, z** — floating-point or integer values

### Return value

1-3,A) If no errors occur, the hypotenuse of a right-angled triangle,
   \(\scriptsize{\sqrt{x^2+y^2} }\)√x2+y2, is returned.

4,B) If no errors occur, the distance from origin in 3D space,
   \(\scriptsize{\sqrt{x^2+y^2+z^2} }\)√x2+y2+z2, is returned.

If a range error due to overflow occurs, `+HUGE_VAL`, `+HUGE_VALF`, or
`+HUGE_VALL` is returned.

If a range error due to underflow occurs, the correct result (after rounding) is
returned.

### Error handling

Errors are reported as specified in `math_errhandling`.

If the implementation supports IEEE floating-point arithmetic (IEC 60559),

- `std::hypot(x, y)`, `std::hypot(y, x)`, and `std::hypot(x, -y)` are
  equivalent.
- if one of the arguments is ±0, `std::hypot(x, y)` is equivalent to `std::fabs`
  called with the non-zero argument.
- if one of the arguments is ±∞, `std::hypot(x, y)` returns +∞ even if the other
  argument is NaN.
- otherwise, if any of the arguments is NaN, NaN is returned.

### Notes

Implementations usually guarantee precision of less than 1 ulp (Unit in the Last
Place — Unit of Least Precision): GNU, BSD.

`std::hypot(x, y)` is equivalent to `std::abs(std::complex<double>(x, y))`.

POSIX specifies that underflow may only occur when both arguments are subnormal
and the correct result is also subnormal (this forbids naive implementations).

Distance between two points `(x1,y1,z1)` and `(x2,y2,z2)` on 3D space can be
calculated using 3-argument overload of `std::hypot` as `std::hypot(x2 - x1, y2
- y1, z2 - z1)`.
*(since C++17)*

The additional overloads are not required to be provided exactly as (A,B). They
only need to be sufficient to ensure that for their first argument `num1`,
second argument `num2` and the optional third argument `num3`:

- If `num1`, `num2` or `num3` has type long double, then
- Otherwise, if `num1`, `num2` and/or `num3` has type double or an integer type,
  then
- Otherwise, if `num1`, `num2` or `num3` has type float, then
*(until C++23)*

If `num1`, `num2` and `num3` have arithmetic types, then
- `std::hypot(num1, num2)` has the same effect as `std::hypot(static_cast</*
  common-floating-point-type */>(num1), static_cast</*
  common-floating-point-type */>(num2))`, and
- `std::hypot(num1, num2, num3)` has the same effect as
  `std::hypot(static_cast</* common-floating-point-type */>(num1),
  static_cast</* common-floating-point-type */>(num2), static_cast</*
  common-floating-point-type */>(num3))`,
where /* common-floating-point-type */ is the floating-point type with the
greatest floating-point conversion rank and greatest floating-point conversion
subrank among the types of `num1`, `num2` and `num3`, arguments of integer type
are considered to have the same floating-point conversion rank as double.
If no such floating-point type with the greatest rank and subrank exists, then
overload resolution does not result in a usable candidate from the overloads
provided.
*(since C++23)*

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_hypot` | 201603L | (C++17) | 3-argument overload of `std::hypot`

### Example

```cpp
#include <cerrno>
#include <cfenv>
#include <cfloat>
#include <cmath>
#include <cstring>
#include <iostream>

// #pragma STDC FENV_ACCESS ON

struct Point3D { float x, y, z; };

int main()
{
    // typical usage
    std::cout << "(1,1) cartesian is (" << std::hypot(1,1)
              << ',' << std::atan2(1,1) << ") polar\n";

    Point3D a{3.14, 2.71, 9.87}, b{1.14, 5.71, 3.87};
    // C++17 has 3-argument hypot overload:
    std::cout << "distance(a,b) = "
              << std::hypot(a.x - b.x, a.y - b.y, a.z - b.z) << '\n';

    // special values
    std::cout << "hypot(NAN,INFINITY) = " << std::hypot(NAN, INFINITY) << '\n';

    // error handling
    errno = 0;
    std::feclearexcept(FE_ALL_EXCEPT);
    std::cout << "hypot(DBL_MAX,DBL_MAX) = " << std::hypot(DBL_MAX, DBL_MAX) << '\n';

    if (errno == ERANGE)
        std::cout << "    errno = ERANGE " << std::strerror(errno) << '\n';
    if (std::fetestexcept(FE_OVERFLOW))
        std::cout << "    FE_OVERFLOW raised\n";
}
```

Output:

```text
(1,1) cartesian is (1.41421,0.785398) polar
distance(a,b) = 7
hypot(NAN,INFINITY) = inf
hypot(DBL_MAX,DBL_MAX) = inf
    errno = ERANGE Numerical result out of range
    FE_OVERFLOW raised
```

### See also

- **powpowfpowl (C++11)(C++11)** — raises a number to the given power
  (\(\small{x^y}\)xy) (function)
- **sqrtsqrtfsqrtl (C++11)(C++11)** — computes square root
  (\(\small{\sqrt{x}}\)√x) (function)
- **cbrtcbrtfcbrtl (C++11)(C++11)(C++11)** — computes cube root
  (\(\small{\sqrt[3]{x}}\)3√x) (function)
- **abs(std::complex)** — returns the magnitude of a complex number (function
  template)

**C documentation for `hypot`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/hypot*
