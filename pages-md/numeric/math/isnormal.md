# std::isnormal

```cpp
bool isnormal( float num );
bool isnormal( double num );
bool isnormal( long double num );  // (since C++11) (until C++23)
constexpr bool isnormal( /* floating-point-type */ num );  // (since C++23)
Additional overloads
template< class Integer >
bool isnormal( Integer num );  // (A) (since C++11) (constexpr since C++23)
```

1) Determines if the given floating point number `num` is normal, i.e. is
   neither zero, subnormal, infinite, nor NaN. The library provides overloads
   for all cv-unqualified floating-point types as the type of the parameter
   `num`.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
   `double`.

### Parameters

- **num** — floating-point or integer value

### Return value

`true` if `num` is normal, `false` otherwise.

### Notes

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their argument `num` of integer
type, `std::isnormal(num)` has the same effect as
`std::isnormal(static_cast<double>(num))`.

### Example

```cpp
#include <cfloat>
#include <cmath>
#include <iostream>

int main()
{
    std::cout << std::boolalpha
              << "isnormal(NaN) = " << std::isnormal(NAN) << '\n'
              << "isnormal(Inf) = " << std::isnormal(INFINITY) << '\n'
              << "isnormal(0.0) = " << std::isnormal(0.0) << '\n'
              << "isnormal(DBL_MIN/2.0) = " << std::isnormal(DBL_MIN / 2.0) << '\n'
              << "isnormal(1.0) = " << std::isnormal(1.0) << '\n';
}
```

Output:

```text
isnormal(NaN) = false
isnormal(Inf) = false
isnormal(0.0) = false
isnormal(DBL_MIN/2.0) = false
isnormal(1.0) = true
```

### See also

- **fpclassify (C++11)** — categorizes the given floating-point value (function)
- **isfinite (C++11)** — checks if the given number has finite value (function)
- **isinf (C++11)** — checks if the given number is infinite (function)
- **isnan (C++11)** — checks if the given number is NaN (function)

**C documentation for `isnormal`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/isnormal*
