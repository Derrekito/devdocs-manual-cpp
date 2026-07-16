# std::exp, std::expf, std::expl

Computes e (`2.71828...`) raised to the power `num`. There's no domain
restriction — every real `num` has a finite or infinite result — but
large positive `num` overflows to `+HUGE_VAL` (`+∞` on IEEE builds) and
very negative `num` underflows toward `+0`. For `double`, overflow is
guaranteed once `num > 709.8`, and underflow is guaranteed once
`num < -708.4`.

```cpp skip
std::exp(num);      // e raised to num, as float/double/long double
std::expf(num);     // float overload                    (since C++11)
std::expl(num);     // long double overload               (since C++11)
```

### What you provide

- **num** — a floating-point or integer value.

### Guarantees and costs

- Range error on overflow: returns `+HUGE_VAL` (matching `+∞`) and
  raises `FE_OVERFLOW`. On underflow, the correctly rounded (near-zero)
  result is returned instead of an error.
- `exp(±0)` is `1`; `exp(-∞)` is `+0`; `exp(+∞)` is `+∞`; `exp(NaN)` is
  `NaN`.
- Errors follow `math_errhandling` — check `errno == ERANGE` and/or
  `std::fetestexcept(FE_OVERFLOW)`, whichever the implementation
  defines.

### Gotchas

- Overflow doesn't throw — it silently returns `+∞` unless you check
  `errno`/`FE_OVERFLOW` yourself, and `+∞` can then propagate through
  later arithmetic without an obvious symptom.
- The overflow/underflow thresholds (roughly ±709 for `double`) are
  easy to hit when exponentiating anything derived from real-world
  units (rates, energies) without first checking the input's range.
- `std::exp(1)` is the standard way to get e as a runtime value; for a
  compile-time constant, prefer `std::numbers::e` (C++20).

### Example

```cpp
#include <cerrno>
#include <cfenv>
#include <cmath>
#include <cstring>
#include <iomanip>
#include <iostream>

int main()
{
    std::cout << std::fixed << std::setprecision(3)
              << "exp(1) = e = " << std::exp(1) << '\n'
              << "FV of $100 at 3% continuous compounding for 1 year = "
              << 100 * std::exp(0.03) << '\n';

    std::cout << "exp(-0) = " << std::exp(-0.0) << '\n'
              << "exp(-Inf) = " << std::exp(-INFINITY) << '\n';

    errno = 0;
    std::feclearexcept(FE_ALL_EXCEPT);

    std::cout << "exp(710) is inf: " << std::isinf(std::exp(710)) << '\n';
    if (errno == ERANGE)
        std::cout << "    errno == ERANGE: " << std::strerror(errno) << '\n';
    if (std::fetestexcept(FE_OVERFLOW))
        std::cout << "    FE_OVERFLOW raised\n";
}
```

```text
exp(1) = e = 2.718
FV of $100 at 3% continuous compounding for 1 year = 103.045
exp(-0) = 1.000
exp(-Inf) = 0.000
exp(710) is inf: 1
    errno == ERANGE: Numerical result out of range
    FE_OVERFLOW raised
```

### Reference

```cpp skip
float       exp ( float num );
double      exp ( double num );
long double exp ( long double num );  // (until C++23)
/* floating-point-type */
            exp ( /* floating-point-type */ num );  // (since C++23) (constexpr since C++26)
float       expf( float num );  // (since C++11) (constexpr since C++26)
long double expl( long double num );  // (since C++11) (constexpr since C++26)
template< class Integer >
double      exp ( Integer num );  // (since C++11) (constexpr since C++26)
```

The integer overload need only behave *as if* `num` were cast to
`double` first, not be implemented exactly as the template above.

### See also

- **exp2** — 2 raised to the given power (C++11)
- **expm1** — e raised to the given power, minus one (C++11)
- **log** — computes natural (base e) logarithm

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/exp*
