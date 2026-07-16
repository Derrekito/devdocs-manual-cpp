# std::fetestexcept

```cpp
int fetestexcept( int excepts );  // (since C++11)
```

Determines which of the specified subset of the floating point exceptions are
currently set. The argument `excepts` is a bitwise OR of the floating point
exception macros.

### Parameters

- **excepts** — bitmask listing the exception flags to test

### Return value

Bitwise OR of the floating-point exception macros that are both included in
`excepts` and correspond to floating-point exceptions currently set.

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

- **feclearexcept (C++11)** — clears the specified floating-point status flags
  (function)

**C documentation for `fetestexcept`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/fenv/fetestexcept*
