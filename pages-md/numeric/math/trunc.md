# std::trunc, std::truncf, std::truncl

Rounds toward zero, dropping the fractional part regardless of sign:
`trunc(2.7)` is `2` and `trunc(-2.7)` is `-2`. This is where it differs
from `std::floor` and `std::ceil`, which always move in one direction
on the number line — `floor(-2.5)` is `-3` and `ceil(2.5)` is `3`, but
`trunc(-2.5)` is `-2` and `trunc(2.5)` is `2`.

```cpp skip
std::trunc(num);     // toward zero, as float/double/long double
std::truncf(num);    // float overload                    (since C++11)
std::truncl(num);    // long double overload               (since C++11)
```

### What you provide

- **num** — a floating-point or integer value.

### Guarantees and costs

- Returns the nearest integer not greater in magnitude than `num` —
  `num` rounded toward zero — still as a floating-point type.
- `±∞` and `±0` are returned unmodified; `NaN` returns `NaN`. The
  current rounding mode has no effect on this function.
- Never overflows on its own (the largest representable floating-point
  values are already exact integers), but the *result* can overflow if
  you store it into a narrower integer type.
- `FE_INEXACT` may (but need not) be raised when truncating a
  non-integer finite value. Errors otherwise follow `math_errhandling`.

### Gotchas

- `trunc` is the one of the three that matches "just drop the decimal
  part" intuition on negative numbers; `floor` and `ceil` don't.
  `trunc(-2.5)` is `-2`, while `floor(-2.5)` is `-3`.
- The implicit conversion from a floating-point type to an integer type
  also rounds toward zero (same direction as `trunc`), but it's UB if
  the value doesn't fit the target type — `trunc` itself never has that
  problem since its result stays floating-point.
- `trunc(-0.9)` is `-0`, not `0` — the sign survives even though the
  magnitude rounds all the way to zero.

### Example

```cpp
#include <cmath>
#include <iomanip>
#include <iostream>

int main()
{
    std::cout << std::fixed << std::setprecision(1)
              << "trunc(+2.7) = " << std::trunc(+2.7) << '\n'
              << "trunc(-2.7) = " << std::trunc(-2.7) << '\n'
              << "trunc(-2.5) = " << std::trunc(-2.5) << '\n'
              << "trunc(-0.9) = " << std::trunc(-0.9) << '\n';
}
```

```text
trunc(+2.7) = 2.0
trunc(-2.7) = -2.0
trunc(-2.5) = -2.0
trunc(-0.9) = -0.0
```

### Reference

```cpp skip
float       trunc ( float num );
double      trunc ( double num );
long double trunc ( long double num );  // (until C++23)
constexpr /* floating-point-type */
            trunc ( /* floating-point-type */ num );  // (since C++23)
float       truncf( float num );  // (since C++11) (constexpr since C++23)
long double truncl( long double num );  // (since C++11) (constexpr since C++23)
template< class Integer >
double      trunc ( Integer num );  // (since C++11) (constexpr since C++23)
```

The integer overload need only behave *as if* `num` were cast to
`double` first, not be implemented exactly as the template above.

### See also

- **floor** — nearest integer not greater than the given value
- **ceil** — nearest integer not less than the given value
- **round** — nearest integer, rounding away from zero on ties (C++11)

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/trunc*
