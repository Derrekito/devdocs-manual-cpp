# std::pow, std::powf, std::powl

Raises `base` to the power `exp`. It is **not** a compile-time or
guaranteed-exact operation: `std::pow(x, 2)` is a general floating-point
routine, not a special-cased square — for an exact square just write
`x * x`. Negative `base` with a non-integer `exp` is a domain error
(there's no real result), and `base == 0` with negative `exp` is a pole
error (the true result is infinite).

```cpp skip
std::pow(base, exp);    // base and exp both floating-point or integer
std::powf(base, exp);   // float overload                  (since C++11)
std::powl(base, exp);   // long double overload             (since C++11)
```

### What you provide

- **base, exp** — floating-point or integer values.

### Guarantees and costs

- Domain error if `base` is finite and negative and `exp` is finite and
  non-integer: returns an implementation-defined value (NaN where
  supported) and raises `FE_INVALID`.
- Pole error if `base` is zero and `exp` is negative: returns `±HUGE_VAL`
  (matching `±∞` when the sign can be determined) and raises
  `FE_DIVBYZERO`.
- `pow(+1, exp)` is always `1`, even if `exp` is NaN; `pow(base, ±0)` is
  always `1`, even if `base` is NaN.
- Overflow on a finite result returns `±HUGE_VAL` (range error);
  underflow returns the correctly rounded result.
- Errors are reported as specified by `math_errhandling` — check `errno`
  (`EDOM`, `ERANGE`) and/or the floating-point exception flags
  (`FE_INVALID`, `FE_DIVBYZERO`), whichever the implementation defines.
- C++98 had a separate `pow(base, int)` overload returning the base's
  own type; C++11's generic overload set instead promotes an `int`
  exponent to `double`, so `pow(float, int)` now returns `double`, not
  `float` (LWG 550 removed the old overload to resolve the conflict).

### Gotchas

- `pow(x, 2)` is not evaluated at compile time and is not guaranteed
  exact — it's a general (and comparatively slow) routine. Prefer
  `x * x` for a literal square.
- A negative base with a fractional exponent (e.g. `pow(-1, 1.0/3)`)
  looks like it should give a real cube root but is a domain error
  instead — `std::pow` doesn't take branch cuts for you. Use
  `std::cbrt` for a real cube root of a negative number.
- Passing two integers still returns a floating-point value (the
  additional overloads for arithmetic types resolve to a floating-point
  common type), so large integer powers can silently lose precision.

### Example

```cpp
#include <cerrno>
#include <cfenv>
#include <cmath>
#include <cstring>
#include <iostream>

int main()
{
    std::cout << "pow(2, 10) = " << std::pow(2, 10) << '\n'
              << "pow(2, 0.5) = " << std::pow(2, 0.5) << '\n'
              << "pow(-2, -3) = " << std::pow(-2, -3) << '\n';

    errno = 0;
    std::feclearexcept(FE_ALL_EXCEPT);

    double r = std::pow(-1, 1.0 / 3);
    std::cout << "pow(-1, 1/3) is nan: " << std::isnan(r) << '\n';
    if (errno == EDOM)
        std::cout << "    errno == EDOM " << std::strerror(errno) << '\n';
    if (std::fetestexcept(FE_INVALID))
        std::cout << "    FE_INVALID raised\n";
}
```

```text
pow(2, 10) = 1024
pow(2, 0.5) = 1.41421
pow(-2, -3) = -0.125
pow(-1, 1/3) is nan: 1
    errno == EDOM Numerical argument out of domain
    FE_INVALID raised
```

### Reference

```cpp skip
float       pow ( float base, float exp );
double      pow ( double base, double exp );
long double pow ( long double base, long double exp );  // (until C++23)
/* floating-point-type */
            pow ( /* floating-point-type */ base,
                  /* floating-point-type */ exp );  // (since C++23) (constexpr since C++26)
float       powf( float base, float exp );  // (since C++11) (constexpr since C++26)
long double powl( long double base, long double exp );  // (since C++11) (constexpr since C++26)
template< class Arithmetic1, class Arithmetic2 >
/* common-floating-point-type */
            pow ( Arithmetic1 base, Arithmetic2 exp );  // (since C++11) (constexpr since C++26)
```

Full IEEE special-value table (signs of zero, ±∞ combinations) is kept
verbatim upstream; the cases most likely to surprise: `pow(-0, exp)`
with negative odd integer `exp` returns `-∞` and raises `FE_DIVBYZERO`;
`pow(-∞, exp)` returns `-0`/`+0`/`-∞`/`+∞` depending on the sign and
parity of `exp`. `std::cbrt` covers the exp = 1/3 case that `pow` can't
handle for negative bases. The additional (arithmetic-type) overloads
need only behave *as if* both arguments were cast to their common
floating-point type, with integers ranked as `double`.

### See also

- **sqrt** — computes the square root
- **cbrt** — computes the cube root, including of negative numbers
- **hypot** — square root of the sum of squares (C++11)

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/pow*
