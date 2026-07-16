# Standard library header <cmath>

`<cmath>` is the C++ wrapper around C's `<math.h>`: everything from
`sqrt` and `sin` to rounding and classification lives here. Every
function comes in three overloads for `float`, `double`, and
`long double` — call `sin(x)` and the right one is picked by
overload resolution; the `f`/`l`-suffixed names (`sinf`, `sinl`, …)
exist for parity with C and to call a specific width explicitly.
Domain and range errors are reported the C way: check
`math_errhandling` — implementations report failures via `errno`
(`MATH_ERRNO`), floating-point exceptions (`MATH_ERREXCEPT`), or
both, rather than throwing.

### Types

- **`float_t`**, **`double_t`** (C++11) — the most efficient
  floating-point type at least as wide as `float` / `double`

### Macros

- **`HUGE_VAL`**, **`HUGE_VALF`**, **`HUGE_VALL`** (C++11) — overflow
  value for `double`, `float`, `long double`
- **`INFINITY`** (C++11) — positive infinity, or the largest value
  guaranteed to overflow `float`
- **`NAN`** (C++11) — a quiet NaN of type `float`
- **`math_errhandling`**, **`MATH_ERRNO`**, **`MATH_ERREXCEPT`**
  (C++11) — which error-reporting mechanism is in effect
- **`FP_NORMAL`**, **`FP_SUBNORMAL`**, **`FP_ZERO`**, **`FP_INFINITE`**,
  **`FP_NAN`** (C++11) — floating-point category constants, returned
  by `fpclassify`

### Basic operations

- **`abs`**, **`fabs`**, **`fabsf`**, **`fabsl`** — absolute value
- **`fmod`**, **`fmodf`**, **`fmodl`** (C++11) — floating-point
  remainder of division
- **`remainder`** family (C++11) — signed IEEE remainder of division
- **`remquo`** family (C++11) — signed remainder plus low bits of the
  quotient
- **`fma`** family (C++11) — fused multiply-add, one rounding
- **`fmax`** / **`fmin`** families (C++11) — larger / smaller of two
  values, NaN-aware
- **`fdim`** family (C++11) — positive difference, `max(0, x-y)`
- **`nan`**, **`nanf`**, **`nanl`** (C++11) — construct a NaN

### Linear interpolation

- **`lerp`** (C++20) — linear interpolation between two values

### Exponential functions

- **`exp`** family (C++11) — *e* raised to a power
- **`exp2`** family (C++11) — 2 raised to a power
- **`expm1`** family (C++11) — `exp(x) - 1`, accurate for small `x`
- **`log`** family (C++11) — natural (base *e*) logarithm
- **`log10`** family (C++11) — common (base 10) logarithm
- **`log2`** family (C++11) — base 2 logarithm
- **`log1p`** family (C++11) — `log(1 + x)`, accurate for small `x`

### Power functions

- **`pow`** family (C++11) — raise a number to a power
- **`sqrt`** family (C++11) — square root
- **`cbrt`** family (C++11) — cube root
- **`hypot`** family (C++11; three-argument overload C++17) —
  hypotenuse, `sqrt(x²+y²)` without intermediate overflow

### Trigonometric functions

- **`sin`**, **`cos`**, **`tan`** families (C++11) — sine, cosine,
  tangent
- **`asin`**, **`acos`**, **`atan`** families (C++11) — inverse
  trigonometric functions
- **`atan2`** family (C++11) — arc tangent using both signs to pick
  the quadrant

### Hyperbolic functions

- **`sinh`**, **`cosh`**, **`tanh`** families (C++11) — hyperbolic
  sine, cosine, tangent
- **`asinh`**, **`acosh`**, **`atanh`** families (C++11) — inverse
  hyperbolic functions

### Error and gamma functions

- **`erf`** / **`erfc`** families (C++11) — error function and its
  complement
- **`tgamma`** family (C++11) — the gamma function
- **`lgamma`** family (C++11) — natural log of the gamma function

### Rounding and nearest-integer operations

- **`ceil`** family — smallest integer not less than the value
- **`floor`** family — largest integer not greater than the value
- **`trunc`** family (C++11) — nearest integer not greater in
  magnitude (rounds toward zero)
- **`round`**, **`lround`**, **`llround`** families (C++11) —
  round to nearest, halfway cases away from zero
- **`nearbyint`** family (C++11) — round using the current rounding
  mode, never raises `FE_INEXACT`
- **`rint`**, **`lrint`**, **`llrint`** families (C++11) — round
  using the current rounding mode, may raise `FE_INEXACT`

### Floating-point manipulation functions

- **`frexp`** family — decompose into significand and power of 2
- **`ldexp`** family — multiply by 2 raised to a power
- **`modf`** family — split into integer and fractional parts
- **`scalbn`**, **`scalbln`** families (C++11) — multiply by
  `FLT_RADIX` raised to a power
- **`ilogb`** family (C++11) — extract the integer exponent
- **`logb`** family (C++11) — extract the exponent as a float
- **`nextafter`**, **`nexttoward`** families (C++11) — the next
  representable value toward a target
- **`copysign`** family (C++11) — copy the sign bit from one value to
  another

### Classification and comparison

- **`fpclassify`** (C++11) — categorize a value (normal, subnormal,
  zero, infinite, NaN)
- **`isfinite`**, **`isinf`**, **`isnan`**, **`isnormal`**,
  **`signbit`** (C++11) — one-question predicates on a value
- **`isgreater`**, **`isgreaterequal`**, **`isless`**,
  **`islessequal`**, **`islessgreater`**, **`isunordered`** (C++11) —
  comparisons that don't raise on NaN, unlike `<`/`>`

### Mathematical special functions (C++17)

Bessel, elliptic, Laguerre, Legendre, and related functions for
numerical/scientific code: **`assoc_laguerre`**, **`assoc_legendre`**,
**`beta`**, **`comp_ellint_1`** / **`_2`** / **`_3`**,
**`cyl_bessel_i`** / **`_j`** / **`_k`**, **`cyl_neumann`**,
**`ellint_1`** / **`_2`** / **`_3`**, **`expint`**, **`hermite`**,
**`laguerre`**, **`legendre`**, **`riemann_zeta`**, **`sph_bessel`**,
**`sph_legendre`**, **`sph_neumann`** — each with `f`/`l` overloads,
all C++17.

---
*Source: https://en.cppreference.com/w/cpp/header/cmath*
