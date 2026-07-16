# std::feholdexcept

```cpp
int feholdexcept( std::fenv_t* envp )  // (since C++11)
```

First, saves the current floating-point environment to the object pointed to by
`envp` (similar to `std::fegetenv`), then clears all floating-point status
flags, and then installs the non-stop mode: future floating-point exceptions
will not interrupt execution (will not trap), until the floating-point
environment is restored by `std::feupdateenv` or `std::fesetenv`.

This function may be used in the beginning of a subroutine that must hide the
floating-point exceptions it may raise from the caller. If only some exceptions
must be suppressed, while others must be reported, the non-stop mode is usually
ended with a call to `std::feupdateenv` after clearing the unwanted exceptions.

### Parameters

- **envp** — pointer to the object of type `std::fenv_t` where the
  floating-point environment will be stored

### Return value

`​0​` on success, non-zero otherwise.

### See also

- **feupdateenv (C++11)** — restores the floating-point environment and raises
  the previously raised exceptions (function)
- **fegetenvfesetenv (C++11)** — saves or restores the current floating-point
  environment (function)
- **FE_DFL_ENV (C++11)** — default floating-point environment (macro constant)

**C documentation for `feholdexcept`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/fenv/feholdexcept*
