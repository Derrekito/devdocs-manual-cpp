# std::round, std::roundf, std::roundl, std::lround, std::lroundf, std::lroundl, std::llround, std::llroundf

```cpp
Rounding to floating-point types
float       round ( float num );
double      round ( double num );
long double round ( long double num );  // (since C++11) (until C++23)
constexpr /* floating-point-type */
            round ( /* floating-point-type */ num );  // (since C++23)
float       roundf( float num );  // (2) (since C++11) (constexpr since C++23)
long double roundl( long double num );  // (3) (since C++11) (constexpr since C++23)
long
long lround ( float num );
long lround ( double num );
long lround ( long double num );  // (since C++11) (until C++23)
constexpr long lround( /* floating-point-type */ num );  // (since C++23)
long lroundf( float num );  // (5) (since C++11) (constexpr since C++23)
long lroundl( long double num );  // (6) (since C++11) (constexpr since C++23)
long long
long long llround ( float num );
long long llround ( double num );
long long llround ( long double num );  // (since C++11) (until C++23)
constexpr long long llround( /* floating-point-type */ num );  // (since C++23)
long long llroundf( float num );  // (8) (since C++11) (constexpr since C++23)
long long llroundl( long double num );  // (9) (since C++11) (constexpr since C++23)
Additional overloads
template< class Integer >
double round( Integer num );  // (A) (since C++11) (constexpr since C++23)
template< class Integer >
long lround( Integer num );  // (B) (since C++11) (constexpr since C++23)
template< class Integer >
long long llround( Integer num );  // (C) (since C++11) (constexpr since C++23)
```

1-3) Computes the nearest integer value to `num` (in floating-point format),
   rounding halfway cases away from zero, regardless of the current rounding
   mode. The library provides overloads of `std::round` for all cv-unqualified
   floating-point types as the type of the parameter `num`.(since C++23)

4-9) Computes the nearest integer value to `num` (in integer format), rounding
   halfway cases away from zero, regardless of the current rounding mode. The
   library provides overloads of `std::lround` and `std::llround` for all
   cv-unqualified floating-point types as the type of the parameter `num`.(since
   C++23)

A-C) Additional overloads are provided for all integer types, which are treated
   as `double`.

### Parameters

- **num** — floating-point or integer value

### Return value

If no errors occur, the nearest integer value to `num`, rounding halfway cases
away from zero, is returned.

Return value

`num`

If a domain error occurs, an implementation-defined value is returned.

### Error handling

Errors are reported as specified in `math_errhandling`.

If the result of `std::lround` or `std::llround` is outside the range
representable by the return type, a domain error or a range error may occur.

If the implementation supports IEEE floating-point arithmetic (IEC 60559), For
the `std::round` function:

- The current rounding mode has no effect.
- If `num` is ±∞, it is returned, unmodified.
- If `num` is ±0, it is returned, unmodified.
- If `num` is NaN, NaN is returned.

For `std::lround` and `std::llround` functions:

- `FE_INEXACT` is never raised.
- The current rounding mode has no effect.
- If `num` is ±∞, `FE_INVALID` is raised and an implementation-defined value is
  returned.
- If the result of the rounding is outside the range of the return type,
  `FE_INVALID` is raised and an implementation-defined value is returned.
- If `num` is NaN, `FE_INVALID` is raised and an implementation-defined value is
  returned.

### Notes

`FE_INEXACT` may be (but is not required to be) raised by `std::round` when
rounding a non-integer finite value.

The largest representable floating-point values are exact integers in all
standard floating-point formats, so `std::round` never overflows on its own;
however the result may overflow any integer type (including `std::intmax_t`),
when stored in an integer variable.

POSIX specifies that all cases where `std::lround` or `std::llround` raise
`FE_INEXACT` are domain errors.

The double version of `std::round` behaves as if implemented as follows:

```cpp
#include <cfenv>
#include <cmath>

#pragma STDC FENV_ACCESS ON

double round(double x)
{
    std::fenv_t save_env;
    std::feholdexcept(&save_env);
    double result = std::rint(x);
    if (std::fetestexcept(FE_INEXACT))
    {
        auto const save_round = std::fegetround();
        std::fesetround(FE_TOWARDZERO);
        result = std::rint(std::copysign(0.5 + std::fabs(x), x));
        std::fesetround(save_round);
    }
    std::feupdateenv(&save_env);
    return result;
}
```

The additional overloads are not required to be provided exactly as (A-C). They
only need to be sufficient to ensure that for their argument `num` of integer
type:

- `std::round(num)` has the same effect as
  `std::round(static_cast<double>(num))`.
- `std::lround(num)` has the same effect as
  `std::lround(static_cast<double>(num))`.
- `std::llround(num)` has the same effect as
  `std::llround(static_cast<double>(num))`.

### Example

```cpp
#include <cfenv>
#include <climits>
#include <cmath>
#include <iostream>

// #pragma STDC FENV_ACCESS ON

int main()
{
    // round
    std::cout << "round(+2.3) = " << std::round(2.3)
              << "  round(+2.5) = " << std::round(2.5)
              << "  round(+2.7) = " << std::round(2.7) << '\n'
              << "round(-2.3) = " << std::round(-2.3)
              << "  round(-2.5) = " << std::round(-2.5)
              << "  round(-2.7) = " << std::round(-2.7) << '\n';

    std::cout << "round(-0.0) = " << std::round(-0.0)  << '\n'
              << "round(-Inf) = " << std::round(-INFINITY) << '\n';

    // lround
    std::cout << "lround(+2.3) = " << std::lround(2.3)
              << "  lround(+2.5) = " << std::lround(2.5)
              << "  lround(+2.7) = " << std::lround(2.7) << '\n'
              << "lround(-2.3) = " << std::lround(-2.3)
              << "  lround(-2.5) = " << std::lround(-2.5)
              << "  lround(-2.7) = " << std::lround(-2.7) << '\n';

    std::cout << "lround(-0.0) = " << std::lround(-0.0)  << '\n'
              << "lround(-Inf) = " << std::lround(-INFINITY) << '\n';

    // error handling
    std::feclearexcept(FE_ALL_EXCEPT);

    std::cout << "std::lround(LONG_MAX+1.5) = "
              << std::lround(LONG_MAX + 1.5) << '\n';
    if (std::fetestexcept(FE_INVALID))
        std::cout << "    FE_INVALID was raised\n";
}
```

Possible output:

```text
round(+2.3) = 2  round(+2.5) = 3  round(+2.7) = 3
round(-2.3) = -2  round(-2.5) = -3  round(-2.7) = -3
round(-0.0) = -0
round(-Inf) = -inf
lround(+2.3) = 2  lround(+2.5) = 3  lround(+2.7) = 3
lround(-2.3) = -2  lround(-2.5) = -3  lround(-2.7) = -3
lround(-0.0) = 0
lround(-Inf) = -9223372036854775808
std::lround(LONG_MAX+1.5) = -9223372036854775808
    FE_INVALID was raised
```

### See also

- **floorfloorffloorl (C++11)(C++11)** — nearest integer not greater than the
  given value (function)
- **ceilceilfceill (C++11)(C++11)** — nearest integer not less than the given
  value (function)
- **trunctruncftruncl (C++11)(C++11)(C++11)** — nearest integer not greater in
  magnitude than the given value (function)

**C documentation for `round`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/round*
