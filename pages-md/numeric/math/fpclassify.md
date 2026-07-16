# std::fpclassify

```cpp
int fpclassify( float num );
int fpclassify( double num );
int fpclassify( long double num );  // (since C++11) (until C++23)
constexpr int fpclassify( /* floating-point-type */ num );  // (since C++23)
Additional overloads
template< class Integer >
int fpclassify( Integer num );  // (A) (since C++11) (constexpr since C++23)
```

1) Categorizes floating point value `num` into the following categories: zero,
   subnormal, normal, infinite, NAN, or implementation-defined category. The
   library provides overloads of `std::fpclassify` for all cv-unqualified
   floating-point types as the type of the parameter `num`.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
   `double`.

### Parameters

- **num** — floating-point or integer value

### Return value

one of `FP_INFINITE`, `FP_NAN`, `FP_NORMAL`, `FP_SUBNORMAL`, `FP_ZERO` or
implementation-defined type, specifying the category of `num`.

### Notes

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their argument `num` of integer
type, `std::fpclassify(num)` has the same effect as
`std::fpclassify(static_cast<double>(num))`.

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

- **isfinite (C++11)** — checks if the given number has finite value (function)
- **isinf (C++11)** — checks if the given number is infinite (function)
- **isnan (C++11)** — checks if the given number is NaN (function)
- **isnormal (C++11)** — checks if the given number is normal (function)
- **numeric_limits** — provides an interface to query properties of all
  fundamental numeric types (class template)

**C documentation for `fpclassify`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/fpclassify*
