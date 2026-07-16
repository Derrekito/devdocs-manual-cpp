# std::isnan

Tests whether `num` is a not-a-number (NaN) value. This exists because
the obvious ways to check don't work: NaN never compares equal to
anything, **including itself** (`NaN == NaN` is `false`), so `x != x`
is a well-known trick for detecting it — but `std::isnan` says the same
thing directly and is the preferred spelling. `std::isnan` is a
function template overload set (unlike the C macro `isnan` it's built
on), so it participates in overload resolution and ADL the way normal
C++ functions do.

```cpp skip
std::isnan(num);     // true if num is NaN
```

### What you provide

- **num** — a floating-point or integer value (integers are always
  `false` — there's no integer NaN).

### Guarantees and costs

- Returns `true` if `num` is a NaN, `false` otherwise.
- `constexpr` since C++23.
- There are many distinct NaN bit patterns (different sign bits and
  payloads, see `std::nan` and `numeric_limits::quiet_NaN`); `isnan`
  doesn't distinguish them — any NaN gives `true`.

### Gotchas

- `x != x` works as a NaN test only because NaN is specifically
  unequal to itself; it relies on IEEE-754 comparison semantics and is
  easy to misread as a no-op or a bug by someone skimming the code —
  prefer `std::isnan(x)`, which says what it means.
- GCC and Clang's `-ffast-math` (via the implied `-ffinite-math`)
  license the compiler to assume NaN can't occur, and under that flag
  `std::isnan` is assumed to always return `false` — code that relies
  on NaN detection must not be built with `-ffast-math`.
- Copying a NaN is not required by IEEE-754 to preserve its exact bit
  pattern (sign and payload), though most implementations do — don't
  rely on a NaN's payload surviving a copy.

### Example

```cpp
#include <cfloat>
#include <cmath>
#include <iostream>

int main()
{
    std::cout << std::boolalpha
              << "isnan(NaN) = " << std::isnan(NAN) << '\n'
              << "isnan(Inf) = " << std::isnan(INFINITY) << '\n'
              << "isnan(0.0) = " << std::isnan(0.0) << '\n'
              << "isnan(0.0 / 0.0) = " << std::isnan(0.0 / 0.0) << '\n'
              << "isnan(Inf - Inf) = " << std::isnan(INFINITY - INFINITY) << '\n';
}
```

```text
isnan(NaN) = true
isnan(Inf) = false
isnan(0.0) = false
isnan(0.0 / 0.0) = true
isnan(Inf - Inf) = true
```

### Reference

```cpp skip
bool isnan( float num );
bool isnan( double num );
bool isnan( long double num );  // (since C++11) (until C++23)
constexpr bool isnan( /* floating-point-type */ num );  // (since C++23)
template< class Integer >
bool isnan( Integer num );  // (since C++11) (constexpr since C++23)
```

The integer overload need only behave *as if* `num` were cast to
`double` first, not be implemented exactly as the template above.

### See also

- **isinf** — checks if the given number is infinite (C++11)
- **isfinite** — checks if the given number has a finite value (C++11)
- **isnormal** — checks if the given number is normal (C++11)
- **fpclassify** — categorizes the given floating-point value (C++11)
- **nan** — generates a quiet NaN (C++11)

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/isnan*
