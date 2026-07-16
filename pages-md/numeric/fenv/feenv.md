# std::fegetenv, std::fesetenv

```cpp
int fegetenv( std::fenv_t* envp )  // (1) (since C++11)
int fesetenv( const std::fenv_t* envp );  // (2) (since C++11)
```

Manages the status of the floating-point environment.

1) Attempts to store the status of the floating-point environment in the object
   pointed to by `envp`.

2) Attempts to establish the floating-point environment from the object pointed
   to by `envp`. The value of that object must be previously obtained by a call
   to `std::feholdexcept` or `std::fegetenv` or be a floating-point macro
   constant. If any of the floating-point status flags are set in `envp`, they
   become set in the environment (and are then testable with
   `std::fetestexcept`), but the corresponding floating-point exceptions are not
   raised (execution continues uninterrupted)

### Parameters

- **envp** — pointer to the object of type `std::fenv_t` which holds the status
  of the floating-point environment

### Return value

`​0​` on success, non-zero otherwise.

### See also

- **feholdexcept (C++11)** — saves the environment, clears all status flags and
  ignores all future errors (function)
- **feupdateenv (C++11)** — restores the floating-point environment and raises
  the previously raised exceptions (function)
- **FE_DFL_ENV (C++11)** — default floating-point environment (macro constant)

**C documentation for `fegetenv, fesetenv`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/fenv/feenv*
