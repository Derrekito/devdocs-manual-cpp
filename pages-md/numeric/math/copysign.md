# std::copysign, std::copysignf, std::copysignl

```cpp
float       copysign ( float mag, float sgn );
double      copysign ( double mag, double sgn );
long double copysign ( long double mag, long double sgn );  // (until C++23)
constexpr /* floating-point-type */
            copysign ( /* floating-point-type */ mag,
                       /* floating-point-type */ sgn );  // (since C++23)
float       copysignf( float mag, float sgn );  // (2) (since C++11) (constexpr since C++23)
long double copysignl( long double mag, long double sgn );  // (3) (since C++11) (constexpr since C++23)
Additional overloads (since C++11)
template< class Integer >
double      copysign ( Integer mag, Integer sgn );  // (A) (constexpr since C++23)
```

1-3) Composes a floating point value with the magnitude of `mag` and the sign of
   `sgn`. The library provides overloads of `std::copysign` for all
   cv-unqualified floating-point types as the type of the parameters.(since
   C++23)

A) Additional overloads are provided for all integer types, which are treated as
double.
*(since C++11)*

### Parameters

- **mag, sgn** — floating-point or integer values

### Return value

If no errors occur, the floating point value with the magnitude of `mag` and the
sign of `sgn` is returned.

If `mag` is NaN, then NaN with the sign of `sgn` is returned.

If `sgn` is -0, the result is only negative if the implementation supports the
signed zero consistently in arithmetic operations.

### Error handling

This function is not subject to any errors specified in `math_errhandling`.

If the implementation supports IEEE floating-point arithmetic (IEC 60559),

- The returned value is exact (`FE_INEXACT` is never raised) and independent of
  the current rounding mode.

### Notes

`std::copysign` is the only portable way to manipulate the sign of a NaN value
(to examine the sign of a NaN, `std::signbit` may also be used).

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their first argument `num1` and
second argument `num2`:

- If `num1` or `num2` has type long double, then `std::copysign(num1, num2)` has
  the same effect as `std::copysign(static_cast<long double>(num1),
  static_cast<long double>(num2))`.
- Otherwise, if `num1` and/or `num2` has type double or an integer type, then
  `std::copysign(num1, num2)` has the same effect as
  `std::copysign(static_cast<double>(num1), static_cast<double>(num2))`.
- Otherwise, if `num1` or `num2` has type float, then `std::copysign(num1,
  num2)` has the same effect as `std::copysign(static_cast<float>(num1),
  static_cast<float>(num2))`.
*(until C++23)*

If `num1` and `num2` have arithmetic types, then `std::copysign(num1, num2)` has
the same effect as `std::copysign(static_cast</* common-floating-point-type
*/>(num1), static_cast</* common-floating-point-type */>(num2))`, where /*
common-floating-point-type */ is the floating-point type with the greatest
floating-point conversion rank and greatest floating-point conversion subrank
between the types of `num1` and `num2`, arguments of integer type are considered
to have the same floating-point conversion rank as double.
If no such floating-point type with the greatest rank and subrank exists, then
overload resolution does not result in a usable candidate from the overloads
provided.
*(since C++23)*

### Example

```cpp
#include <cmath>
#include <iostream>

int main()
{
    std::cout << std::showpos
              << "copysign(1.0,+2.0) = " << std::copysign(1.0, +2.0) << '\n'
              << "copysign(1.0,-2.0) = " << std::copysign(1.0, -2.0) << '\n'
              << "copysign(inf,-2.0) = " << std::copysign(INFINITY, -2.0) << '\n'
              << "copysign(NaN,-2.0) = " << std::copysign(NAN, -2.0) << '\n';
}
```

Output:

```text
copysign(1.0,+2.0) = +1
copysign(1.0,-2.0) = -1
copysign(inf,-2.0) = -inf
copysign(NaN,-2.0) = -nan
```

### See also

- **abs(float)fabsfabsffabsl (C++11)(C++11)** — absolute value of a floating
  point value (\(\small{|x|}\)|x|) (function)
- **signbit (C++11)** — checks if the given number is negative (function)

**C documentation for `copysign`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/copysign*
