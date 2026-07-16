# MATH_ERRNO, MATH_ERREXCEPT, math_errhandling

```cpp
#define MATH_ERRNO        1  // (since C++11)
#define MATH_ERREXCEPT    2  // (since C++11)
#define math_errhandling  /*implementation defined*/  // (since C++11)
```

The macro constant `math_errhandling` expands to an expression of type int that
is either equal to `MATH_ERRNO`, or equal to `MATH_ERREXCEPT`, or equal to their
bitwise OR (`MATH_ERRNO | MATH_ERREXCEPT`).

The value of `math_errhandling` indicates the type of error handling that is
performed by the floating-point operators and functions:

- **`MATH_ERREXCEPT`** — Indicates that floating-point exceptions are used: at
  least `FE_DIVBYZERO`, `FE_INVALID`, and `FE_OVERFLOW` are defined in
  `<cfenv>`.
- **`MATH_ERRNO`** — Indicates that floating-point operations use the variable
  `errno` to report errors.

If the implementation supports IEEE floating-point arithmetic (IEC 60559),
`math_errhandling & MATH_ERREXCEPT` is required to be non-zero.

The following floating-point error conditions are recognized:

  Condition | Explanation | errno | Floating-point exception | Example
  Domain error | The argument is outside the range in which the operation is
      mathematically defined (the description of each function lists the
      required domain errors) | `EDOM` | `FE_INVALID` | `std::acos(2)`
  Pole error | The mathematical result of the function is exactly infinite or
      undefined | `ERANGE` | `FE_DIVBYZERO` | `std::log(0.0)`, `1.0 / 0.0`
  Range error due to overflow | The mathematical result is finite, but becomes
      infinite after rounding, or becomes the largest representable finite value
      after rounding down | `ERANGE` | `FE_OVERFLOW` | `std::pow(DBL_MAX, 2)`
  Range error due to underflow | The result is non-zero, but becomes zero after
      rounding, or becomes subnormal with a loss of precision | `ERANGE` or
      unchanged (implementation-defined) | `FE_UNDERFLOW` or nothing
      (implementation-defined) | `DBL_TRUE_MIN / 2`
  Inexact result | The result has to be rounded to fit in the destination type |
      Unchanged | `FE_INEXACT` or nothing (unspecified) | `std::sqrt(2)`, `1.0 /
      10.0`

### Notes

Whether `FE_INEXACT` is raised by the mathematical library functions is
unspecified in general, but may be explicitly specified in the description of
the function (e.g. `std::rint` vs `std::nearbyint`).

Before C++11, floating-point exceptions were not specified, `EDOM` was required
for any domain error, `ERANGE` was required for overflows and
implementation-defined for underflows.

### Example

```cpp
#include <cerrno>
#include <cfenv>
#include <cmath>
#include <cstring>
#include <iostream>
// #pragma STDC FENV_ACCESS ON

int main()
{
    std::cout << "MATH_ERRNO is "
              << (math_errhandling & MATH_ERRNO ? "set" : "not set") << '\n'
              << "MATH_ERREXCEPT is "
              << (math_errhandling & MATH_ERREXCEPT ? "set" : "not set") << '\n';
    std::feclearexcept(FE_ALL_EXCEPT);
    errno = 0;
    std::cout <<  "log(0) = " << std::log(0) << '\n';
    if (errno == ERANGE)
        std::cout << "errno = ERANGE (" << std::strerror(errno) << ")\n";
    if (std::fetestexcept(FE_DIVBYZERO))
        std::cout << "FE_DIVBYZERO (pole error) reported\n";
}
```

Possible output:

```text
MATH_ERRNO is set
MATH_ERREXCEPT is set
log(0) = -inf
errno = ERANGE (Numerical result out of range)
FE_DIVBYZERO (pole error) reported
```

### See also

- **FE_ALL_EXCEPTFE_DIVBYZEROFE_INEXACTFE_INVALIDFE_OVERFLOWFE_UNDERFLOW
  (C++11)** — floating-point exceptions (macro constant)
- **errno** — macro which expands to POSIX-compatible thread-local error number
  variable (macro variable)

**C documentation for `math_errhandling`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/math_errhandling*
