# Floating-point environment

The floating-point environment is the set of floating-point status flags and
control modes supported by the implementation. It is thread-local. Each thread
inherits the initial state of its floating-point environment from the parent
thread. Floating-point operations modify the floating-point status flags to
indicate abnormal results or auxiliary information. The state of floating-point
control modes affects the outcomes of some floating-point operations.

The floating-point environment access and modification is only meaningful when
`#pragma STDC FENV_ACCESS` is supported and is set to `ON`. Otherwise the
implementation is free to assume that floating-point control modes are always
the default ones and that floating-point status flags are never tested or
modified. In practice, few current compilers, such as HP aCC, Oracle Studio, or
IBM XL, support the `#pragma` explicitly, but most compilers allow meaningful
access to the floating-point environment anyway.

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

### Notes

The floating-point exceptions are not related to the C++ exceptions. When a
floating-point operation raises a floating-point exception, the status of the
floating-point environment changes, which can be tested with
`std::fetestexcept`, but the execution of a C++ program on most implementations
continues uninterrupted.

There are compiler extensions that may be used to generate C++ exceptions
automatically whenever a floating-point exception is raised:

- GNU libc function `feenableexcept()` enables trapping of the floating-point
  exceptions, which generates the signal `SIGFPE`. If the compiler option
  `-fnon-call-exceptions` was used, the handler for that signal may throw a
  user-defined C++ exception.
- MSVC function `_control87()` enables trapping of the floating-point
  exceptions, which generates a hardware exception, which can be converted to
  C++ exceptions with `_set_se_translator`.

### See also

**C documentation for `Floating-point environment`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/fenv*
