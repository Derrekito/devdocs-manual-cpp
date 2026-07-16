# std::fmod, std::fmodf, std::fmodl

Computes the floating-point remainder of `x / y`, defined as
`x - trunc(x / y) * y`. The result always has the **same sign as `x`**
(the dividend) and is less than `y` in magnitude — this is different
from `std::remainder`, which rounds the quotient to the *nearest*
integer instead of truncating, and so can return a result with either
sign, independent of `x`'s sign.

```cpp skip
std::fmod(x, y);     // remainder, sign follows x
std::fmodf(x, y);    // float overload                    (since C++11)
std::fmodl(x, y);    // long double overload               (since C++11)
```

### What you provide

- **x, y** — floating-point or integer values.

### Guarantees and costs

- Domain error may occur if `y` is zero.
- If `x` is `±0` and `y` is nonzero, returns `±0`. If `x` is `±∞` (and
  `y` isn't NaN), or `y` is `±0` (and `x` isn't NaN), returns NaN and
  raises `FE_INVALID`. If `y` is `±∞` and `x` is finite, `x` is returned
  unchanged. If either argument is NaN, NaN is returned.
- POSIX additionally treats "`x` infinite or `y` zero" as a domain
  error in all cases.
- Errors follow `math_errhandling` — check `errno` and/or
  `FE_INVALID`, whichever the implementation defines.

### Gotchas

- The result's sign always follows `x`, never `y`: `fmod(5.1, -3)` is
  `+2.1`, not `-2.1`. If you need a result with the same sign as `y`
  (or the smallest-magnitude remainder), use `std::remainder` instead.
- `fmod(x, 0)` is a domain error (NaN, `FE_INVALID`), unlike integer
  division by zero it doesn't crash — but the NaN can silently
  propagate if you don't check for it.
- `x - std::trunc(x / y) * y` is the mathematical definition but isn't
  a safe substitute for `std::fmod`: computing `x / y` first can lose
  precision that `std::fmod` avoids.

### Example

```cpp
#include <cfenv>
#include <cmath>
#include <iostream>

int main()
{
    std::cout << "fmod(+5.1, +3.0) = " << std::fmod(5.1, 3) << '\n'
              << "fmod(-5.1, +3.0) = " << std::fmod(-5.1, 3) << '\n'
              << "fmod(+5.1, -3.0) = " << std::fmod(5.1, -3) << '\n'
              << "fmod(-5.1, -3.0) = " << std::fmod(-5.1, -3) << '\n';

    std::feclearexcept(FE_ALL_EXCEPT);
    double r = std::fmod(5.1, 0);
    std::cout << "fmod(+5.1, 0) is nan: " << std::isnan(r) << '\n';
    if (std::fetestexcept(FE_INVALID))
        std::cout << "    FE_INVALID raised\n";
}
```

```text
fmod(+5.1, +3.0) = 2.1
fmod(-5.1, +3.0) = -2.1
fmod(+5.1, -3.0) = 2.1
fmod(-5.1, -3.0) = -2.1
fmod(+5.1, 0) is nan: 1
    FE_INVALID raised
```

### Reference

```cpp skip
float       fmod ( float x, float y );
double      fmod ( double x, double y );
long double fmod ( long double x, long double y );  // (until C++23)
constexpr /* floating-point-type */
            fmod ( /* floating-point-type */ x,
                   /* floating-point-type */ y );  // (since C++23)
float       fmodf( float x, float y );  // (since C++11) (constexpr since C++23)
long double fmodl( long double x, long double y );  // (since C++11) (constexpr since C++23)
template< class Integer >
double      fmod ( Integer x, Integer y );  // (since C++11) (constexpr since C++23)
```

The additional overloads need only behave *as if* both arguments were
cast to their common floating-point type, with integers ranked as
`double`.

### See also

- **remainder** — signed remainder rounded to nearest, not truncated (C++11)
- **remquo** — signed remainder plus low bits of the quotient (C++11)
- **div** — quotient and remainder of integer division (C++11)

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/fmod*
