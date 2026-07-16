# FP_NORMAL, FP_SUBNORMAL, FP_ZERO, FP_INFINITE, FP_NAN

```cpp
#define FP_NORMAL    /* implementation defined */  // (since C++11)
#define FP_SUBNORMAL /* implementation defined */  // (since C++11)
#define FP_ZERO      /* implementation defined */  // (since C++11)
#define FP_INFINITE  /* implementation defined */  // (since C++11)
#define FP_NAN       /* implementation defined */  // (since C++11)
```

The `FP_NORMAL`, `FP_SUBNORMAL`, `FP_ZERO`, `FP_INFINITE`, `FP_NAN` macros each
represent a distinct category of floating-point numbers. They all expand to an
integer constant expression.

- **`FP_NORMAL`** — indicates that the value is *normal*, i.e. not an infinity,
  subnormal, not-a-number or zero
- **`FP_SUBNORMAL`** — indicates that the value is *subnormal*
- **`FP_ZERO`** — indicates that the value is positive or negative zero
- **`FP_INFINITE`** — indicates that the value is not representable by the
  underlying type (positive or negative infinity)
- **`FP_NAN`** — indicates that the value is not-a-number (NaN)

### Example

```cpp
#include <cfloat>
#include <cmath>
#include <iostream>

auto show_classification(double x)
{
    switch (std::fpclassify(x))
    {
        case FP_INFINITE:
            return "Inf";
        case FP_NAN:
            return "NaN";
        case FP_NORMAL:
            return "normal";
        case FP_SUBNORMAL:
            return "subnormal";
        case FP_ZERO:
            return "zero";
        default:
            return "unknown";
    }
}

int main()
{
    std::cout << "1.0/0.0 is " << show_classification(1 / 0.0) << '\n'
              << "0.0/0.0 is " << show_classification(0.0 / 0.0) << '\n'
              << "DBL_MIN/2 is " << show_classification(DBL_MIN / 2) << '\n'
              << "-0.0 is " << show_classification(-0.0) << '\n'
              << "1.0 is " << show_classification(1.0) << '\n';
}
```

Output:

```text
1.0/0.0 is Inf
0.0/0.0 is NaN
DBL_MIN/2 is subnormal
-0.0 is zero
1.0 is normal
```

### See also

- **fpclassify (C++11)** — categorizes the given floating-point value (function)

**C documentation for `FP_categories`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/FP_categories*
