# std::fmod, std::fmodf, std::fmodl

```cpp
float       fmod ( float x, float y );
double      fmod ( double x, double y );
long double fmod ( long double x, long double y );  // (until C++23)
constexpr /* floating-point-type */
            fmod ( /* floating-point-type */ x,
                   /* floating-point-type */ y );  // (since C++23)
float       fmodf( float x, float y );  // (2) (since C++11) (constexpr since C++23)
long double fmodl( long double x, long double y );  // (3) (since C++11) (constexpr since C++23)
Additional overloads (since C++11)
template< class Integer >
double      fmod ( Integer x, Integer y );  // (A) (constexpr since C++23)
```

1-3) Computes the floating-point remainder of the division operation `x / y`.
   The library provides overloads of `std::fmod` for all cv-unqualified
   floating-point types as the type of the parameters.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
double.
*(since C++11)*

The floating-point remainder of the division operation `x / y` calculated by
this function is exactly the value `x - rem * y`, where `rem` is `x / y` with
its fractional part truncated.

The returned value has the same sign as `x` and is less than `y` in magnitude.

### Parameters

- **x, y** — floating-point or integer values

### Return value

If successful, returns the floating-point remainder of the division `x / y` as
defined above.

If a domain error occurs, an implementation-defined value is returned (NaN where
supported).

If a range error occurs due to underflow, the correct result (after rounding) is
returned.

### Error handling

Errors are reported as specified in `math_errhandling`.

Domain error may occur if `y` is zero.

If the implementation supports IEEE floating-point arithmetic (IEC 60559),

- If `x` is ±0 and `y` is not zero, ±0 is returned.
- If `x` is ±∞ and `y` is not NaN, NaN is returned and `FE_INVALID` is raised.
- If `y` is ±0 and `x` is not NaN, NaN is returned and `FE_INVALID` is raised.
- If `y` is ±∞ and `x` is finite, `x` is returned.
- If either argument is NaN, NaN is returned.

### Notes

POSIX requires that a domain error occurs if `x` is infinite or `y` is zero.

`std::fmod`, but not `std::remainder` is useful for doing silent wrapping of
floating-point types to unsigned integer types: `(0.0 <= (y =
std::fmod(std::rint(x), 65536.0)) ? y : 65536.0 + y)` is in the range
`[``-0.0``,``65535.0``]`, which corresponds to unsigned short, but
`std::remainder(std::rint(x), 65536.0` is in the range
`[``-32767.0``,``+32768.0``]`, which is outside of the range of signed short.

The double version of `std::fmod` behaves as if implemented as follows:

```cpp
double fmod(double x, double y)
{
#pragma STDC FENV_ACCESS ON
    double result = std::remainder(std::fabs(x), y = std::fabs(y));
    if (std::signbit(result))
        result += y;
    return std::copysign(result, x);
}
```

The expression `x - std::trunc(x / y) * y` may not equal `std::fmod(x, y)`, when
the rounding of `x / y` to initialize the argument of `std::trunc` loses too
much precision (example: `x = 30.508474576271183309`, `y =
6.1016949152542370172`).

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their first argument `num1` and
second argument `num2`:

- If `num1` or `num2` has type long double, then `std::fmod(num1, num2)` has the
  same effect as `std::fmod(static_cast<long double>(num1), static_cast<long
  double>(num2))`.
- Otherwise, if `num1` and/or `num2` has type double or an integer type, then
  `std::fmod(num1, num2)` has the same effect as
  `std::fmod(static_cast<double>(num1), static_cast<double>(num2))`.
- Otherwise, if `num1` or `num2` has type float, then `std::fmod(num1, num2)`
  has the same effect as `std::fmod(static_cast<float>(num1),
  static_cast<float>(num2))`.
*(until C++23)*

If `num1` and `num2` have arithmetic types, then `std::fmod(num1, num2)` has the
same effect as `std::fmod(static_cast</* common-floating-point-type */>(num1),
static_cast</* common-floating-point-type */>(num2))`, where /*
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
#include <cfenv>
#include <cmath>
#include <iostream>
// #pragma STDC FENV_ACCESS ON

int main()
{
    std::cout << "fmod(+5.1, +3.0) = " << std::fmod(5.1, 3) << '\n'
              << "fmod(-5.1, +3.0) = " << std::fmod(-5.1, 3) << '\n'
              << "fmod(+5.1, -3.0) = " << std::fmod(5.1, -3) << '\n'
              << "fmod(-5.1, -3.0) = " << std::fmod(-5.1, -3) << '\n';

    // special values
    std::cout << "fmod(+0.0, 1.0) = " << std::fmod(0, 1) << '\n'
              << "fmod(-0.0, 1.0) = " << std::fmod(-0.0, 1) << '\n'
              << "fmod(5.1, Inf) = " << std::fmod(5.1, INFINITY) << '\n';

    // error handling
    std::feclearexcept(FE_ALL_EXCEPT);
    std::cout << "fmod(+5.1, 0) = " << std::fmod(5.1, 0) << '\n';
    if (std::fetestexcept(FE_INVALID))
        std::cout << "    FE_INVALID raised\n";
}
```

Possible output:

```text
fmod(+5.1, +3.0) = 2.1
fmod(-5.1, +3.0) = -2.1
fmod(+5.1, -3.0) = 2.1
fmod(-5.1, -3.0) = -2.1
fmod(+0.0, 1.0) = 0
fmod(-0.0, 1.0) = -0
fmod(5.1, Inf) = 5.1
fmod(+5.1, 0) = -nan
    FE_INVALID raised
```

### See also

- **div(int)ldivlldiv (C++11)** — computes quotient and remainder of integer
  division (function)
- **remainderremainderfremainderl (C++11)(C++11)(C++11)** — signed remainder of
  the division operation (function)
- **remquoremquofremquol (C++11)(C++11)(C++11)** — signed remainder as well as
  the three last bits of the division operation (function)

**C documentation for `fmod`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/fmod*
