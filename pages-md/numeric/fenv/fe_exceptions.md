# FE_DIVBYZERO, FE_INEXACT, FE_INVALID, FE_OVERFLOW, FE_UNDERFLOW, FE_ALL_EXCEPT

```cpp
#define FE_DIVBYZERO    /*implementation defined power of 2*/  // (since C++11)
#define FE_INEXACT      /*implementation defined power of 2*/  // (since C++11)
#define FE_INVALID      /*implementation defined power of 2*/  // (since C++11)
#define FE_OVERFLOW     /*implementation defined power of 2*/  // (since C++11)
#define FE_UNDERFLOW    /*implementation defined power of 2*/  // (since C++11)
#define FE_ALL_EXCEPT  FE_DIVBYZERO | FE_INEXACT | \
                       FE_INVALID | FE_OVERFLOW |  \
                       FE_UNDERFLOW  // (since C++11)
```

All these macro constants (except `FE_ALL_EXCEPT`) expand to integer constant
expressions that are distinct powers of 2, which uniquely identify all supported
floating-point exceptions. Each macro is defined only if it is supported.

The macro constant `FE_ALL_EXCEPT`, which expands to the bitwise OR of all other
`FE_*`, is always defined and is zero if floating-point exceptions are not
supported by the implementation.

- **`FE_DIVBYZERO`** — pole error occurred in an earlier floating-point
  operation
- **`FE_INEXACT`** — inexact result: rounding was necessary to store the result
  of an earlier floating-point operation
- **`FE_INVALID`** — domain error occurred in an earlier floating-point
  operation
- **`FE_OVERFLOW`** — the result of the earlier floating-point operation was too
  large to be representable
- **`FE_UNDERFLOW`** — the result of the earlier floating-point operation was
  subnormal with a loss of precision
- **`FE_ALL_EXCEPT`** — bitwise OR of all supported floating-point exceptions

The implementation may define additional macro constants in `<cfenv>` to
identify additional floating-point exceptions. All such constants begin with
`FE_` followed by at least one uppercase letter.

See `math_errhandling` for further details.

### Example

```cpp
#include <cfenv>
#include <cmath>
#include <iostream>

// #pragma STDC FENV_ACCESS ON

volatile double zero = 0.0; // volatile not needed where FENV_ACCESS is supported
volatile double one = 1.0;  // volatile not needed where FENV_ACCESS is supported

int main()
{
    std::feclearexcept(FE_ALL_EXCEPT);
    std::cout <<  "1.0/0.0 = " << 1.0 / zero << '\n';
    if (std::fetestexcept(FE_DIVBYZERO))
        std::cout << "division by zero reported\n";
    else
        std::cout << "division by zero not reported\n";

    std::feclearexcept(FE_ALL_EXCEPT);
    std::cout << "1.0/10 = " << one / 10 << '\n';
    if (std::fetestexcept(FE_INEXACT))
        std::cout << "inexact result reported\n";
    else
        std::cout << "inexact result not reported\n";

    std::feclearexcept(FE_ALL_EXCEPT);
    std::cout << "sqrt(-1) = " << std::sqrt(-1) << '\n';
    if (std::fetestexcept(FE_INVALID))
        std::cout << "invalid result reported\n";
    else
        std::cout << "invalid result not reported\n";
}
```

Possible output:

```text
1.0/0.0 = inf
division by zero reported
1.0/10 = 0.1
inexact result reported
sqrt(-1) = -nan
invalid result reported
```

### See also

- **math_errhandlingMATH_ERRNOMATH_ERREXCEPT (C++11)(C++11)(C++11)** — defines
  the error handling mechanism used by the common mathematical functions (macro
  constant)

**C documentation for floating point exception macros**

---
*Source: https://en.cppreference.com/w/cpp/numeric/fenv/FE_exceptions*
