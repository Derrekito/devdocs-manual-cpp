# FE_DFL_ENV

```cpp
#define FE_DFL_ENV /*implementation defined*/  // (since C++11)
```

The macro constant `FE_DFL_ENV` expands to an expression of type `const
std::fenv_t*`, which points to a full copy of the default floating-point
environment, that is, the environment as loaded at program startup.

Additional macros that begin with `FE_` followed by uppercase letters, and have
the type `const std::fenv_t*`, may be supported by an implementation.

### Example

```cpp
#include <cfenv>
#include <iostream>
// #pragma STDC FENV_ACCESS ON

void show_env()
{
    const int e = std::fetestexcept(FE_ALL_EXCEPT);
    if (e & FE_DIVBYZERO)
        std::cout << "division by zero is raised\n";
    if (e & FE_INEXACT)
        std::cout << "inexact is raised\n";
    if (e & FE_INVALID)
        std::cout << "invalid is raised\n";
    if (e & FE_UNDERFLOW)
        std::cout << "underflow is raised\n";
    if (e & FE_OVERFLOW)
        std::cout << "overflow is raised\n";

    switch (std::fegetround())
    {
        case FE_DOWNWARD:
            std::cout << "rounding down\n";
            break;
        case FE_TONEAREST:
            std::cout << "rounding to nearest\n";
            break;
        case FE_TOWARDZERO:
            std::cout << "rounding to zero\n";
            break;
        case FE_UPWARD:
            std::cout << "rounding up\n";
            break;
    }
}

int main()
{
    std::cout << "On startup:\n";
    show_env();

    std::feraiseexcept(FE_UNDERFLOW | FE_OVERFLOW);
    std::fesetround(FE_UPWARD);

    std::cout << "\nBefore restoration:\n";
    show_env();

    std::fesetenv(FE_DFL_ENV);

    std::cout << "\nAfter reset to default:\n";
    show_env();
}
```

Output:

```text
On startup:
rounding to nearest

Before restoration:
underflow is raised
overflow is raised
rounding up

After reset to default:
rounding to nearest
```

### See also

- **fegetenvfesetenv (C++11)** — saves or restores the current floating-point
  environment (function)
- **feupdateenv (C++11)** — restores the floating-point environment and raises
  the previously raised exceptions (function)

**C documentation for `FE_DFL_ENV`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/fenv/FE_DFL_ENV*
