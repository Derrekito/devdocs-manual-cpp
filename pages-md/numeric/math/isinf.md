# std::isinf

```cpp
bool isinf( float num );
bool isinf( double num );
bool isinf( long double num );  // (since C++11) (until C++23)
constexpr bool isinf( /* floating-point-type */ num );  // (since C++23)
Additional overloads
template< class Integer >
bool isinf( Integer num );  // (A) (since C++11) (constexpr since C++23)
```

1) Determines if the given floating-point number `num` is a positive or negative
   infinity. The library provides overloads for all cv-unqualified
   floating-point types as the type of the parameter `num`.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
   `double`.

### Parameters

- **num** — floating-point or integer value

### Return value

`true` if `num` is infinite, `false` otherwise.

### Notes

GCC and Clang support a `-ffinite-math` option (additionally implied by
`-ffast-math`), which allows the respective compiler to assume the nonexistence
of special IEEE-754 floating point values such as NaN, infinity, or negative
zero. In other words, `std::isinf` is assumed to always return `false` under
this option.

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their argument `num` of integer
type, `std::isinf(num)` has the same effect as
`std::isinf(static_cast<double>(num))`.

### Example

```cpp
#include <cfloat>
#include <cmath>
#include <iostream>
#include <limits>

int main()
{
    const double max = std::numeric_limits<double>::max();
    const double inf = std::numeric_limits<double>::infinity();

    std::cout << std::boolalpha
              << "isinf(NaN) = " << std::isinf(NAN) << '\n'
              << "isinf(Inf) = " << std::isinf(INFINITY) << '\n'
              << "isinf(max) = " << std::isinf(max) << '\n'
              << "isinf(inf) = " << std::isinf(inf) << '\n'
              << "isinf(0.0) = " << std::isinf(0.0) << '\n'
              << "isinf(exp(800)) = " << std::isinf(std::exp(800)) << '\n'
              << "isinf(DBL_MIN/2.0) = " << std::isinf(DBL_MIN / 2.0) << '\n';
}
```

Output:

```text
isinf(NaN) = false
isinf(Inf) = true
isinf(max) = false
isinf(inf) = true
isinf(0.0) = false
isinf(exp(800)) = true
isinf(DBL_MIN/2.0) = false
```

### See also

- **fpclassify (C++11)** — categorizes the given floating-point value (function)
- **isfinite (C++11)** — checks if the given number has finite value (function)
- **isnan (C++11)** — checks if the given number is NaN (function)
- **isnormal (C++11)** — checks if the given number is normal (function)

**C documentation for `isinf`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/isinf*
