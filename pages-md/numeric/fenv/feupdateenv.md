# std::feupdateenv

```cpp
int feupdateenv( const std::fenv_t* envp )  // (since C++11)
```

First, remembers the currently raised floating-point exceptions, then restores
the floating-point environment from the object pointed to by `envp` (similar to
`std::fesetenv`), then raises the floating-point exceptions that were saved.

This function may be used to end the non-stop mode established by an earlier
call to `std::feholdexcept`.

### Parameters

- **envp** — pointer to the object of type `std::fenv_t` set by an earlier call
  to `std::feholdexcept` or `std::fegetenv` or equal to `FE_DFL_ENV`

### Return value

`​0​` on success, non-zero otherwise.

### See also

- **feholdexcept (C++11)** — saves the environment, clears all status flags and
  ignores all future errors (function)
- **fegetenvfesetenv (C++11)** — saves or restores the current floating-point
  environment (function)
- **FE_DFL_ENV (C++11)** — default floating-point environment (macro constant)

**C documentation for `feupdateenv`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/fenv/feupdateenv*
