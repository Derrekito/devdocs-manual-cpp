# std::isinf

Tests whether `num` is positive or negative infinity. Unlike NaN,
infinity compares normally (`INFINITY == INFINITY` is `true`), so `x
!= x` doesn't help here — `std::isinf` is the direct way to ask the
question, and it's a proper overloaded function (built on the C
`isinf` macro), so it works correctly with overload resolution and
ADL the way ordinary C++ functions do.

```cpp skip
std::isinf(num);     // true if num is +Inf or -Inf
```

### What you provide

- **num** — a floating-point or integer value (integers are always
  `false` — there's no integer infinity).

### Guarantees and costs

- Returns `true` if `num` is infinite (either sign), `false` otherwise.
- `constexpr` since C++23.

### Gotchas

- GCC and Clang's `-ffast-math` (via the implied `-ffinite-math`)
  license the compiler to assume infinities can't occur, and under
  that flag `std::isinf` is assumed to always return `false` — code
  that relies on infinity detection must not be built with
  `-ffast-math`.
- A value that overflows during computation (e.g. `std::exp` of a
  large argument) becomes `+∞` silently; `std::isinf` is how you catch
  that after the fact, but checking `errno`/`FE_OVERFLOW` at the call
  site catches it earlier.
- `isinf(x)` and `!isfinite(x)` are not quite the same question:
  `!isfinite(x)` is also `true` for NaN, while `isinf(x)` is `false`
  for NaN.

### Example

```cpp
#include <cfloat>
#include <cmath>
#include <iostream>
#include <limits>

int main()
{
    const double max = std::numeric_limits<double>::max();
    const double inf = std::numeric_limits<double>::infinity();

    std::cout << std::boolalpha
              << "isinf(NaN) = " << std::isinf(NAN) << '\n'
              << "isinf(Inf) = " << std::isinf(INFINITY) << '\n'
              << "isinf(max) = " << std::isinf(max) << '\n'
              << "isinf(inf) = " << std::isinf(inf) << '\n'
              << "isinf(0.0) = " << std::isinf(0.0) << '\n'
              << "isinf(exp(800)) = " << std::isinf(std::exp(800)) << '\n';
}
```

```text
isinf(NaN) = false
isinf(Inf) = true
isinf(max) = false
isinf(inf) = true
isinf(0.0) = false
isinf(exp(800)) = true
```

### Reference

```cpp skip
bool isinf( float num );
bool isinf( double num );
bool isinf( long double num );  // (since C++11) (until C++23)
constexpr bool isinf( /* floating-point-type */ num );  // (since C++23)
template< class Integer >
bool isinf( Integer num );  // (since C++11) (constexpr since C++23)
```

The integer overload need only behave *as if* `num` were cast to
`double` first, not be implemented exactly as the template above.

### See also

- **isnan** — checks if the given number is NaN (C++11)
- **isfinite** — checks if the given number has a finite value (C++11)
- **isnormal** — checks if the given number is normal (C++11)
- **fpclassify** — categorizes the given floating-point value (C++11)

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/isinf*
