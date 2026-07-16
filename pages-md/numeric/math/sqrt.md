# std::sqrt, std::sqrtf, std::sqrtl

Computes the square root of `num`. For `num < 0` there's no real
result, so it's a domain error: the function returns NaN (where the
implementation supports it) and, on IEEE builds, raises `FE_INVALID`.
`std::sqrt` is one of the few `<cmath>` functions required to be
correctly rounded — the exact mathematical result is returned whenever
it's representable.

```cpp skip
std::sqrt(num);     // float/double/long double, or promoted from int
std::sqrtf(num);    // float overload                    (since C++11)
std::sqrtl(num);    // long double overload               (since C++11)
```

### What you provide

- **num** — a floating-point or integer value.

### Guarantees and costs

- Domain error if `num < 0`: returns an implementation-defined value
  (NaN where supported); on IEEE builds this also raises `FE_INVALID`.
- `+∞` and `±0` are returned unmodified; `NaN` returns `NaN`.
- Errors follow `math_errhandling` — check `errno == EDOM` and/or
  `std::fetestexcept(FE_INVALID)`, whichever the implementation uses.
- Correctly rounded from the infinitely precise result — like the
  arithmetic operators and `std::fma`, but unlike most of `<cmath>`
  (including `std::pow`), which isn't held to that standard.

### Gotchas

- `sqrt(-0.0)` is well-defined (returns `-0`, not an error) — only
  values strictly less than `-0` are a domain error, so don't confuse
  negative zero with an actual negative argument.
- Passing an `int` still works (promoted to `double`), but a negative
  int silently produces NaN rather than failing to compile.
- Checking `errno` requires clearing it first (`errno = 0`) since the
  library never resets it to zero on success.

### Example

```cpp
#include <cerrno>
#include <cfenv>
#include <cmath>
#include <cstring>
#include <iostream>

int main()
{
    std::cout << "sqrt(100) = " << std::sqrt(100) << '\n'
              << "sqrt(2) = " << std::sqrt(2) << '\n'
              << "golden ratio = " << (1 + std::sqrt(5)) / 2 << '\n';

    errno = 0;
    std::feclearexcept(FE_ALL_EXCEPT);

    double r = std::sqrt(-1);
    std::cout << "sqrt(-1.0) is nan: " << std::isnan(r) << '\n';
    if (errno == EDOM)
        std::cout << "    errno = EDOM " << std::strerror(errno) << '\n';
    if (std::fetestexcept(FE_INVALID))
        std::cout << "    FE_INVALID raised\n";
}
```

```text
sqrt(100) = 10
sqrt(2) = 1.41421
golden ratio = 1.61803
sqrt(-1.0) is nan: 1
    errno = EDOM Numerical argument out of domain
    FE_INVALID raised
```

### Reference

```cpp skip
float       sqrt ( float num );
double      sqrt ( double num );
long double sqrt ( long double num );  // (until C++23)
/* floating-point-type */
            sqrt ( /* floating-point-type */ num );  // (since C++23) (constexpr since C++26)
float       sqrtf( float num );  // (since C++11) (constexpr since C++26)
long double sqrtl( long double num );  // (since C++11) (constexpr since C++26)
template< class Integer >
double      sqrt ( Integer num );  // (since C++11) (constexpr since C++26)
```

The integer overload need only behave *as if* `num` were cast to
`double` first, not be implemented exactly as the template above.

### See also

- **pow** — raises a number to a given power
- **cbrt** — computes the cube root (C++11)
- **hypot** — square root of the sum of squares (C++11)

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/sqrt*
