# std::floor, std::floorf, std::floorl

Rounds down to the largest integer not greater than `num` — for
positive values this looks like truncation, but for negative values it
moves *away* from zero: `floor(2.7)` is `2`, while `floor(-2.7)` is
`-3`, not `-2`. Contrast with `std::ceil` (rounds up, toward `+∞`) and
`std::trunc` (always rounds toward zero, so `trunc(-2.7)` is `-2`).

```cpp skip
std::floor(num);     // largest integer <= num, as float/double/long double
std::floorf(num);    // float overload                    (since C++11)
std::floorl(num);    // long double overload               (since C++11)
```

### What you provide

- **num** — a floating-point or integer value.

### Guarantees and costs

- Returns the largest integer value not greater than `num` (⌊num⌋),
  still as a floating-point type — the current rounding mode has no
  effect on this function.
- `±∞` and `±0` are returned unmodified; `NaN` returns `NaN`.
- Never overflows on its own (the largest representable floating-point
  values are already exact integers), but the *result* can overflow if
  you store it into a narrower integer type.
- `FE_INEXACT` may (but need not) be raised when rounding a non-integer
  finite value. Errors otherwise follow `math_errhandling`.

### Gotchas

- On negative values `floor` rounds *away* from zero, the opposite of
  what "round down toward zero" would suggest: `floor(-2.5)` is `-3`.
  If you want rounding toward zero regardless of sign, use
  `std::trunc` instead.
- `floor(-0.0)` prints as `-0` — the sign is preserved even though the
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
              << "floor(+2.7) = " << std::floor(+2.7) << '\n'
              << "floor(-2.7) = " << std::floor(-2.7) << '\n'
              << "floor(-2.5) = " << std::floor(-2.5) << '\n'
              << "floor(-0.0) = " << std::floor(-0.0) << '\n';
}
```

```text
floor(+2.7) = 2.0
floor(-2.7) = -3.0
floor(-2.5) = -3.0
floor(-0.0) = -0.0
```

### Reference

```cpp skip
float       floor ( float num );
double      floor ( double num );
long double floor ( long double num );  // (until C++23)
constexpr /* floating-point-type */
            floor ( /* floating-point-type */ num );  // (since C++23)
float       floorf( float num );  // (since C++11) (constexpr since C++23)
long double floorl( long double num );  // (since C++11) (constexpr since C++23)
template< class Integer >
double      floor ( Integer num );  // (since C++11) (constexpr since C++23)
```

The integer overload need only behave *as if* `num` were cast to
`double` first, not be implemented exactly as the template above.

### See also

- **ceil** — nearest integer not less than the given value
- **trunc** — nearest integer not greater in magnitude than the value
- **round** — nearest integer, rounding away from zero on ties (C++11)

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/floor*
