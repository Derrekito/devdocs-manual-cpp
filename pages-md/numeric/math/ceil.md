# std::ceil, std::ceilf, std::ceill

Rounds up to the smallest integer not less than `num` — for negative
values this looks like truncation, but for positive values it moves
*away* from zero: `ceil(2.4)` is `3`, while `ceil(-2.4)` is `-2`, not
`-3`. Contrast with `std::floor` (rounds down, toward `-∞`) and
`std::trunc` (always rounds toward zero, so `trunc(2.4)` is also `2`
but `trunc(-2.7)` is `-2` where `floor(-2.7)` is `-3`).

```cpp skip
std::ceil(num);      // smallest integer >= num, as float/double/long double
std::ceilf(num);     // float overload                    (since C++11)
std::ceill(num);     // long double overload               (since C++11)
```

### What you provide

- **num** — a floating-point or integer value.

### Guarantees and costs

- Returns the smallest integer value not less than `num` (⌈num⌉), still
  as a floating-point type — the current rounding mode has no effect on
  this function.
- `±∞` and `±0` are returned unmodified; `NaN` returns `NaN`.
- Never overflows on its own (the largest representable floating-point
  values are already exact integers), but the *result* can overflow if
  you store it into a narrower integer type — this is why the return
  type is floating-point, not integral.
- `FE_INEXACT` may (but need not) be raised when rounding a non-integer
  finite value. Errors otherwise follow `math_errhandling`.

### Gotchas

- On positive values `ceil` rounds *away* from zero, the opposite of
  what "round up toward zero" would suggest: `ceil(2.4)` is `3`. If you
  want rounding toward zero regardless of sign, use `std::trunc`
  instead.
- `ceil(-0.0)` prints as `-0` — the sign is preserved even though the
  value is already an integer.
- The result is still a floating-point type; casting it to an integer
  type is a separate step and can overflow if the value is out of
  range for that type.

### Example

```cpp
#include <cmath>
#include <iomanip>
#include <iostream>

int main()
{
    std::cout << std::fixed << std::setprecision(1)
              << "ceil(+2.4) = " << std::ceil(+2.4) << '\n'
              << "ceil(-2.4) = " << std::ceil(-2.4) << '\n'
              << "ceil(-2.5) = " << std::ceil(-2.5) << '\n'
              << "ceil(-0.0) = " << std::ceil(-0.0) << '\n';
}
```

```text
ceil(+2.4) = 3.0
ceil(-2.4) = -2.0
ceil(-2.5) = -2.0
ceil(-0.0) = -0.0
```

### Reference

```cpp skip
float       ceil ( float num );
double      ceil ( double num );
long double ceil ( long double num );  // (until C++23)
constexpr /* floating-point-type */
            ceil ( /* floating-point-type */ num );  // (since C++23)
float       ceilf( float num );  // (since C++11) (constexpr since C++23)
long double ceill( long double num );  // (since C++11) (constexpr since C++23)
template< class Integer >
double      ceil ( Integer num );  // (since C++11) (constexpr since C++23)
```

The integer overload need only behave *as if* `num` were cast to
`double` first, not be implemented exactly as the template above. For
`double`, `ceil` behaves as if implemented via `fesetround(FE_UPWARD)`
followed by `std::rint`.

### See also

- **floor** — nearest integer not greater than the given value
- **trunc** — nearest integer not greater in magnitude than the value
- **round** — nearest integer, rounding away from zero on ties (C++11)
- **nearbyint** — nearest integer using the current rounding mode (C++11)

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/ceil*
