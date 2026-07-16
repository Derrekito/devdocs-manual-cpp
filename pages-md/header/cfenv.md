# Standard library header <cfenv> (C++11)

This header was originally in the C standard library as `<fenv.h>`.

This header is part of the floating-point environment library.

### Types

- **`fenv_t`** — The type representing the entire floating-point environment
- **`fexcept_t`** — The type representing all floating-point status flags
  collectively

### Functions

- **feclearexcept (C++11)** — clears the specified floating-point status flags
  (function)
- **fetestexcept (C++11)** — determines which of the specified floating-point
  status flags are set (function)
- **feraiseexcept (C++11)** — raises the specified floating-point exceptions
  (function)
- **fegetexceptflagfesetexceptflag (C++11)(C++11)** — copies the state of the
  specified floating-point status flags from or to the floating-point
  environment (function)
- **fegetroundfesetround (C++11)(C++11)** — gets or sets rounding direction
  (function)
- **fegetenvfesetenv (C++11)** — saves or restores the current floating-point
  environment (function)
- **feholdexcept (C++11)** — saves the environment, clears all status flags and
  ignores all future errors (function)
- **feupdateenv (C++11)** — restores the floating-point environment and raises
  the previously raised exceptions (function)

### Macros

- **FE_ALL_EXCEPTFE_DIVBYZEROFE_INEXACTFE_INVALIDFE_OVERFLOWFE_UNDERFLOW
  (C++11)** — floating-point exceptions (macro constant)
- **FE_DOWNWARDFE_TONEARESTFE_TOWARDZEROFE_UPWARD (C++11)** — floating-point
  rounding direction (macro constant)
- **FE_DFL_ENV (C++11)** — default floating-point environment (macro constant)

### Synopsis

```cpp
namespace std {
    // types
    typedef /* object type */ fenv_t;
    typedef /* object type */ fexcept_t;

    // functions
    int feclearexcept(int except);
    int fegetexceptflag(fexcept_t *pflag, int except);
    int feraiseexcept(int except);
    int fesetexceptflag(const fexcept_t *pflag, int except);
    int fetestexcept(int except);
    int fegetround(void);
    int fesetround(int mode);
    int fegetenv(fenv_t *penv);
    int feholdexcept(fenv_t *penv);
    int fesetenv(const fenv_t *penv);
    int feupdateenv(const fenv_t *penv);
}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3905 | C++11 | `std::fexcept_t` must be an integer type | it is an object
      type

---
*Source: https://en.cppreference.com/w/cpp/header/cfenv*
