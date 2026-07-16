# Common mathematical functions

### Functions

- **abs(int)labsllabs (C++11)** — computes absolute value of an integral value
  (\(\small{|x|}\)|x|) (function)
- **div(int)ldivlldiv (C++11)** — computes quotient and remainder of integer
  division (function)
- **abs(std::intmax_t)imaxabs (C++11)(C++11)** — computes absolute value of an
  integral value (\(\small{|x|}\)|x|) (function)
- **div(std::intmax_t)imaxdiv (C++11)(C++11)** — computes quotient and remainder
  of integer division (function)

**Basic operations**

- **abs(float)fabsfabsffabsl (C++11)(C++11)** — absolute value of a floating
  point value (\(\small{|x|}\)|x|) (function)
- **fmodfmodffmodl (C++11)(C++11)** — remainder of the floating point division
  operation (function)
- **remainderremainderfremainderl (C++11)(C++11)(C++11)** — signed remainder of
  the division operation (function)
- **remquoremquofremquol (C++11)(C++11)(C++11)** — signed remainder as well as
  the three last bits of the division operation (function)
- **fmafmaffmal (C++11)(C++11)(C++11)** — fused multiply-add operation
  (function)
- **fmaxfmaxffmaxl (C++11)(C++11)(C++11)** — larger of two floating-point values
  (function)
- **fminfminffminl (C++11)(C++11)(C++11)** — smaller of two floating point
  values (function)
- **fdimfdimffdiml (C++11)(C++11)(C++11)** — positive difference of two floating
  point values (\({\small\max{(0, x-y)}}\)max(0, x-y)) (function)
- **nannanfnanl (C++11)(C++11)(C++11)** — not-a-number (NaN) (function)

**Exponential functions**

- **expexpfexpl (C++11)(C++11)** — returns *e* raised to the given power
  (\({\small e^x}\)ex) (function)
- **exp2exp2fexp2l (C++11)(C++11)(C++11)** — returns *2* raised to the given
  power (\({\small 2^x}\)2x) (function)
- **expm1expm1fexpm1l (C++11)(C++11)(C++11)** — returns *e* raised to the given
  power, minus one (\({\small e^x-1}\)ex-1) (function)
- **loglogflogl (C++11)(C++11)** — computes natural (base *e*) logarithm
  (\({\small\ln{x}}\)ln(x)) (function)
- **log10log10flog10l (C++11)(C++11)** — computes common (base *10*) logarithm
  (\({\small\log_{10}{x}}\)log10(x)) (function)
- **log2log2flog2l (C++11)(C++11)(C++11)** — base 2 logarithm of the given
  number (\({\small\log_{2}{x}}\)log2(x)) (function)
- **log1plog1pflog1pl (C++11)(C++11)(C++11)** — natural logarithm (to base *e*)
  of 1 plus the given number (\({\small\ln{(1+x)}}\)ln(1+x)) (function)

**Power functions**

- **powpowfpowl (C++11)(C++11)** — raises a number to the given power
  (\(\small{x^y}\)xy) (function)
- **sqrtsqrtfsqrtl (C++11)(C++11)** — computes square root
  (\(\small{\sqrt{x}}\)√x) (function)
- **cbrtcbrtfcbrtl (C++11)(C++11)(C++11)** — computes cube root
  (\(\small{\sqrt[3]{x}}\)3√x) (function)
- **hypothypotfhypotl (C++11)(C++11)(C++11)** — computes square root of the sum
  of the squares of two or three(since C++17) given numbers
  (\(\scriptsize{\sqrt{x^2+y^2}}\)√x2+y2),
  (\(\scriptsize{\sqrt{x^2+y^2+z^2}}\)√x2+y2+z2)(since C++17) (function)

**Trigonometric functions**

- **sinsinfsinl (C++11)(C++11)** — computes sine (\({\small\sin{x}}\)sin(x))
  (function)
- **coscosfcosl (C++11)(C++11)** — computes cosine (\({\small\cos{x}}\)cos(x))
  (function)
- **tantanftanl (C++11)(C++11)** — computes tangent (\({\small\tan{x}}\)tan(x))
  (function)
- **asinasinfasinl (C++11)(C++11)** — computes arc sine
  (\({\small\arcsin{x}}\)arcsin(x)) (function)
- **acosacosfacosl (C++11)(C++11)** — computes arc cosine
  (\({\small\arccos{x}}\)arccos(x)) (function)
- **atanatanfatanl (C++11)(C++11)** — computes arc tangent
  (\({\small\arctan{x}}\)arctan(x)) (function)
- **atan2atan2fatan2l (C++11)(C++11)** — arc tangent, using signs to determine
  quadrants (function)

**Hyperbolic functions**

- **sinhsinhfsinhl (C++11)(C++11)** — computes hyperbolic sine
  (\({\small\sinh{x}}\)sinh(x)) (function)
- **coshcoshfcoshl (C++11)(C++11)** — computes hyperbolic cosine
  (\({\small\cosh{x}}\)cosh(x)) (function)
- **tanhtanhftanhl (C++11)(C++11)** — computes hyperbolic tangent
  (\({\small\tanh{x}}\)tanh(x)) (function)
- **asinhasinhfasinhl (C++11)(C++11)(C++11)** — computes the inverse hyperbolic
  sine (\({\small\operatorname{arsinh}{x}}\)arsinh(x)) (function)
- **acoshacoshfacoshl (C++11)(C++11)(C++11)** — computes the inverse hyperbolic
  cosine (\({\small\operatorname{arcosh}{x}}\)arcosh(x)) (function)
- **atanhatanhfatanhl (C++11)(C++11)(C++11)** — computes the inverse hyperbolic
  tangent (\({\small\operatorname{artanh}{x}}\)artanh(x)) (function)

**Error and gamma functions**

- **erferfferfl (C++11)(C++11)(C++11)** — error function (function)
- **erfcerfcferfcl (C++11)(C++11)(C++11)** — complementary error function
  (function)
- **tgammatgammaftgammal (C++11)(C++11)(C++11)** — gamma function (function)
- **lgammalgammaflgammal (C++11)(C++11)(C++11)** — natural logarithm of the
  gamma function (function)

**Nearest integer floating point operations**

- **ceilceilfceill (C++11)(C++11)** — nearest integer not less than the given
  value (function)
- **floorfloorffloorl (C++11)(C++11)** — nearest integer not greater than the
  given value (function)
- **trunctruncftruncl (C++11)(C++11)(C++11)** — nearest integer not greater in
  magnitude than the given value (function)
- **roundroundfroundllroundlroundflroundlllroundllroundfllroundl
  (C++11)(C++11)(C++11)(C++11)(C++11)(C++11)(C++11)(C++11)(C++11)** — nearest
  integer, rounding away from zero in halfway cases (function)
- **nearbyintnearbyintfnearbyintl (C++11)(C++11)(C++11)** — nearest integer
  using current rounding mode (function)
- **rintrintfrintllrintlrintflrintlllrintllrintfllrintl
  (C++11)(C++11)(C++11)(C++11)(C++11)(C++11)(C++11)(C++11)(C++11)** — nearest
  integer using current rounding mode with exception if the result differs
  (function)

**Floating point manipulation functions**

- **frexpfrexpffrexpl (C++11)(C++11)** — decomposes a number into significand
  and a power of `2` (function)
- **ldexpldexpfldexpl (C++11)(C++11)** — multiplies a number by `2` raised to a
  power (function)
- **modfmodffmodfl (C++11)(C++11)** — decomposes a number into integer and
  fractional parts (function)
- **scalbnscalbnfscalbnlscalblnscalblnfscalblnl
  (C++11)(C++11)(C++11)(C++11)(C++11)(C++11)** — multiplies a number by
  `FLT_RADIX` raised to a power (function)
- **ilogbilogbfilogbl (C++11)(C++11)(C++11)** — extracts exponent of the number
  (function)
- **logblogbflogbl (C++11)(C++11)(C++11)** — extracts exponent of the number
  (function)
- **nextafternextafterfnextafterlnexttowardnexttowardfnexttowardl
  (C++11)(C++11)(C++11)(C++11)(C++11)(C++11)** — next representable
  floating-point value towards the given value (function)
- **copysigncopysignfcopysignl (C++11)(C++11)(C++11)** — copies the sign of a
  floating point value (function)

**Classification and comparison**

- **fpclassify (C++11)** — categorizes the given floating-point value (function)
- **isfinite (C++11)** — checks if the given number has finite value (function)
- **isinf (C++11)** — checks if the given number is infinite (function)
- **isnan (C++11)** — checks if the given number is NaN (function)
- **isnormal (C++11)** — checks if the given number is normal (function)
- **signbit (C++11)** — checks if the given number is negative (function)
- **isgreater (C++11)** — checks if the first floating-point argument is greater
  than the second (function)
- **isgreaterequal (C++11)** — checks if the first floating-point argument is
  greater or equal than the second (function)
- **isless (C++11)** — checks if the first floating-point argument is less than
  the second (function)
- **islessequal (C++11)** — checks if the first floating-point argument is less
  or equal than the second (function)
- **islessgreater (C++11)** — checks if the first floating-point argument is
  less or greater than the second (function)
- **isunordered (C++11)** — checks if two floating-point values are unordered
  (function)

### Types

- **div_t** — structure type, returned by `std::div` (typedef)
- **ldiv_t** — structure type, returned by `std::ldiv` (typedef)
- **lldiv_t (C++11)** — structure type, returned by `std::lldiv` (typedef)
- **imaxdiv_t (C++11)** — structure type, returned by `std::imaxdiv` (typedef)
- **float_t (C++11)** — most efficient floating-point type at least as wide as
  float (typedef)
- **double_t (C++11)** — most efficient floating-point type at least as wide as
  double (typedef)

### Macro constants

- **HUGE_VALFHUGE_VALHUGE_VALL (C++11)(C++11)** — indicates the overflow value
  for float, double and long double respectively (macro constant)
- **INFINITY (C++11)** — evaluates to positive infinity or the value guaranteed
  to overflow a float (macro constant)
- **NAN (C++11)** — evaluates to a quiet NaN of type float (macro constant)
- **math_errhandlingMATH_ERRNOMATH_ERREXCEPT (C++11)(C++11)(C++11)** — defines
  the error handling mechanism used by the common mathematical functions (macro
  constant)

**Classification**

- **FP_NORMALFP_SUBNORMALFP_ZEROFP_INFINITEFP_NAN
  (C++11)(C++11)(C++11)(C++11)(C++11)** — indicates a floating-point category
  (macro constant)

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_constexpr_cmath` | 202202L | (C++23) | Constexpr for FP environment
      agnostic mathematical functions in `<cmath>` and `<cstdlib>`
  202306L | (C++26) | Constexpr for most mathematical functions in `<cmath>`

### See also

****Mathematical special functions****

**C documentation for Common mathematical functions**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math*
