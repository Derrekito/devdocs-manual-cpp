# std::round, std::lround, std::llround (and f/l variants)

Rounds to the nearest integer, with halfway cases (`x.5`) always
rounding **away from zero** — `2.5` becomes `3`, `-2.5` becomes `-3` —
regardless of the current floating-point rounding mode. `round` and
its `f`/`l` variants return a floating-point value; `lround`/`llround`
return `long`/`long long` directly.

```cpp skip
std::round(num);     // nearest integer, as a float/double/long double
std::lround(num);    // nearest integer, as a long           (since C++11)
std::llround(num);   // nearest integer, as a long long       (since C++11)
```

Overloads for all floating-point types exist directly (since C++23,
`constexpr`); integer arguments are treated as `double`.

### What you provide

- **num** — a floating-point or integer value.

### Guarantees and costs

- Halfway cases always round away from zero, independent of
  `std::fesetround`.
- `±∞` and `±0` are returned unmodified by `round`; `NaN` returns
  `NaN`.
- `round` itself never overflows — the largest representable
  floating-point values are already exact integers — but the *result*
  can overflow if you then store it into a narrower integer type.
- `lround`/`llround`: if `num` is `±∞`, is `NaN`, or the rounded
  result doesn't fit the return type, `FE_INVALID` is raised and an
  implementation-defined value is returned (no exception is thrown —
  check `std::fetestexcept(FE_INVALID)` if this matters). `FE_INEXACT`
  is never raised by these two, unlike `round`, which may (but need
  not) raise it for a non-integer input.
- Error reporting follows `math_errhandling`, as with the rest of
  `<cmath>`.

### Gotchas

- `lround`/`llround` don't throw on overflow — they set a floating-point
  exception flag and return an implementation-defined value. Code that
  expects an exception or a saturated value will silently get garbage
  instead unless it explicitly tests `FE_INVALID`.
- `round(-0.0)` prints as `-0`, and `round` on a value like `-0.4`
  also yields `-0` — the sign is preserved even though the magnitude
  rounds to zero.
- "Rounds away from zero" is not "rounds up" — `round(-2.5)` is `-3`,
  not `-2`.

### Example

```cpp
#include <iostream>
#include <cmath>

int main()
{
    std::cout << "round(+2.3) = " << std::round(2.3)
              << "  round(+2.5) = " << std::round(2.5)
              << "  round(+2.7) = " << std::round(2.7) << '\n'
              << "round(-2.3) = " << std::round(-2.3)
              << "  round(-2.5) = " << std::round(-2.5)
              << "  round(-2.7) = " << std::round(-2.7) << '\n';

    std::cout << "lround(+2.5) = " << std::lround(2.5)
              << "  lround(-2.5) = " << std::lround(-2.5) << '\n';
}
```

```text
round(+2.3) = 2  round(+2.5) = 3  round(+2.7) = 3
round(-2.3) = -2  round(-2.5) = -3  round(-2.7) = -3
lround(+2.5) = 3  lround(-2.5) = -3
```

### Reference

```cpp skip
float       round ( float num );
double      round ( double num );
long double round ( long double num );  // (until C++23)
constexpr /* floating-point-type */
            round ( /* floating-point-type */ num );  // (since C++23)
float       roundf( float num );  // (since C++11) (constexpr since C++23)
long double roundl( long double num );  // (since C++11) (constexpr since C++23)

long lround ( float num );
long lround ( double num );
long lround ( long double num );  // (until C++23)
constexpr long lround( /* floating-point-type */ num );  // (since C++23)
long lroundf( float num );  // (since C++11) (constexpr since C++23)
long lroundl( long double num );  // (since C++11) (constexpr since C++23)

long long llround ( float num );
long long llround ( double num );
long long llround ( long double num );  // (until C++23)
constexpr long long llround( /* floating-point-type */ num );  // (since C++23)
long long llroundf( float num );  // (since C++11) (constexpr since C++23)
long long llroundl( long double num );  // (since C++11) (constexpr since C++23)

template< class Integer >
double round( Integer num );  // (since C++11) (constexpr since C++23)
template< class Integer >
long lround( Integer num );  // (since C++11) (constexpr since C++23)
template< class Integer >
long long llround( Integer num );  // (since C++11) (constexpr since C++23)
```

POSIX additionally specifies that every case where `lround`/`llround`
would raise `FE_INEXACT` is treated as a domain error. The integer
overloads are only required to behave *as if* `num` were cast to
`double` first, not to be implemented exactly as the template above.

### See also

- **floor** — nearest integer not greater than the given value
  (`floorf`, `floorl`, C++11)
- **ceil** — nearest integer not less than the given value (`ceilf`,
  `ceill`, C++11)
- **trunc** — nearest integer not greater in magnitude than the given
  value (`truncf`, `truncl`, C++11)

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/round*
