# std::feraiseexcept

```cpp
int feraiseexcept( int excepts );  // (since C++11)
```

Attempts to raise all floating point exceptions listed in `excepts` (a bitwise
OR of the floating point exception macros). If one of the exceptions is
`FE_OVERFLOW` or `FE_UNDERFLOW`, this function may additionally raise
`FE_INEXACT`. The order in which the exceptions are raised is unspecified,
except that `FE_OVERFLOW` and `FE_UNDERFLOW` are always raised before
`FE_INEXACT`.

### Parameters

- **excepts** — bitmask listing the exception flags to raise

### Return value

`​0​` if all listed exceptions were raised, non-zero value otherwise.

### Example

```cpp
#include <cfenv>
#include <iostream>

// #pragma STDC FENV_ACCESS ON

int main()
{
    std::feclearexcept(FE_ALL_EXCEPT);
    const int r = std::feraiseexcept(FE_UNDERFLOW | FE_DIVBYZERO);
    std::cout << "Raising divbyzero and underflow simultaneously "
              << (r ? "fails" : "succeeds") << " and results in\n";

    const int e = std::fetestexcept(FE_ALL_EXCEPT);
    if (e & FE_DIVBYZERO)
        std::cout << "division by zero\n";
    if (e & FE_INEXACT)
        std::cout << "inexact\n";
    if (e & FE_INVALID)
        std::cout << "invalid\n";
    if (e & FE_UNDERFLOW)
        std::cout << "underflow\n";
    if (e & FE_OVERFLOW)
        std::cout << "overflow\n";
}
```

Output:

```text
Raising divbyzero and underflow simultaneously succeeds and results in
division by zero
underflow
```

### See also

- **feclearexcept (C++11)** — clears the specified floating-point status flags
  (function)
- **fetestexcept (C++11)** — determines which of the specified floating-point
  status flags are set (function)

**C documentation for `feraiseexcept`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/fenv/feraiseexcept*
