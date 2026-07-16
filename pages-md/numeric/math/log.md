# std::log, std::logf, std::logl

Computes the natural (base-*e*) logarithm of `num`. Negative arguments
have no real logarithm, so it's a domain error (returns NaN); `num == 0`
is a pole — the true result is `-∞`, which the function returns while
raising `FE_DIVBYZERO`. For other bases see `std::log10` (base 10),
`std::log2` (base 2), and `std::log1p` (natural log of `1 + num`,
accurate for small `num`).

```cpp skip
std::log(num);      // natural log, as float/double/long double
std::logf(num);     // float overload                    (since C++11)
std::logl(num);     // long double overload               (since C++11)
```

### What you provide

- **num** — a floating-point or integer value.

### Guarantees and costs

- Domain error if `num < 0`: returns an implementation-defined value
  (NaN where supported) and raises `FE_INVALID`.
- Pole error if `num == 0`: returns `-HUGE_VAL` (matching `-∞`) and
  raises `FE_DIVBYZERO`.
- `log(1)` is `+0`; `log(+∞)` is `+∞`; `log(NaN)` is `NaN`.
- Errors follow `math_errhandling` — check `errno` (`ERANGE` for the
  pole, `EDOM` for the domain error) and/or the exception flags
  (`FE_DIVBYZERO`, `FE_INVALID`), whichever the implementation defines.

### Gotchas

- `log(0)` doesn't throw or silently return `0` — it returns `-∞` and
  sets `errno`/raises `FE_DIVBYZERO`; unchecked, that `-∞` propagates
  through later arithmetic.
- A negative argument gives NaN rather than a complex result; use
  `std::log(std::complex<T>{...})` if you actually need one.
- Changing log base by dividing (`std::log(x) / std::log(b)`) is
  correct but does two library calls and two roundings — prefer
  `std::log10`/`std::log2` directly when the base is 10 or 2.

### Example

```cpp
#include <cerrno>
#include <cfenv>
#include <cmath>
#include <cstring>
#include <iostream>

int main()
{
    std::cout << "log(1) = " << std::log(1) << '\n'
              << "base-5 log of 125 = " << std::log(125) / std::log(5) << '\n';

    errno = 0;
    std::feclearexcept(FE_ALL_EXCEPT);

    std::cout << "log(0) = " << std::log(0) << '\n';
    if (errno == ERANGE)
        std::cout << "    errno == ERANGE: " << std::strerror(errno) << '\n';
    if (std::fetestexcept(FE_DIVBYZERO))
        std::cout << "    FE_DIVBYZERO raised\n";
}
```

```text
log(1) = 0
base-5 log of 125 = 3
log(0) = -inf
    errno == ERANGE: Numerical result out of range
    FE_DIVBYZERO raised
```

### Reference

```cpp skip
float       log ( float num );
double      log ( double num );
long double log ( long double num );  // (until C++23)
/* floating-point-type */
            log ( /* floating-point-type */ num );  // (since C++23) (constexpr since C++26)
float       logf( float num );  // (since C++11) (constexpr since C++26)
long double logl( long double num );  // (since C++11) (constexpr since C++26)
template< class Integer >
double      log ( Integer num );  // (since C++11) (constexpr since C++26)
```

The integer overload need only behave *as if* `num` were cast to
`double` first, not be implemented exactly as the template above.

### See also

- **log10** — common (base 10) logarithm (C++11)
- **log2** — base 2 logarithm (C++11)
- **log1p** — natural logarithm of 1 plus the given number (C++11)
- **exp** — returns e raised to the given power

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/log*
