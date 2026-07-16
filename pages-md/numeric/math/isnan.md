# std::isnan

```cpp
bool isnan( float num );
bool isnan( double num );
bool isnan( long double num );  // (since C++11) (until C++23)
constexpr bool isnan( /* floating-point-type */ num );  // (since C++23)
Additional overloads
template< class Integer >
bool isnan( Integer num );  // (A) (since C++11) (constexpr since C++23)
```

1) Determines if the given floating point number `num` is a not-a-number (NaN)
   value. The library provides overloads for all cv-unqualified floating-point
   types as the type of the parameter `num`.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
   `double`.

### Parameters

- **num** — floating-point or integer value

### Return value

`true` if `num` is a NaN, `false` otherwise.

### Notes

There are many different NaN values with different sign bits and payloads, see
`std::nan` and `std::numeric_limits::quiet_NaN`.

NaN values never compare equal to themselves or to other NaN values. Copying a
NaN is not required, by IEEE-754, to preserve its bit representation (sign and
payload), though most implementation do.

Another way to test if a floating-point value is NaN is to compare it with
itself: `bool is_nan(double x) { return x != x; }`.

GCC and Clang support a `-ffinite-math` option (additionally implied by
`-ffast-math`), which allows the respective compiler to assume the nonexistence
of special IEEE-754 floating point values such as NaN, infinity, or negative
zero. In other words, `std::isnan` is assumed to always return `false` under
this option.

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their argument `num` of integer
type, `std::isnan(num)` has the same effect as
`std::isnan(static_cast<double>(num))`.

### Example

```cpp
#include <cfloat>
#include <cmath>
#include <iostream>

int main()
{
    std::cout << std::boolalpha
              << "isnan(NaN) = " << std::isnan(NAN) << '\n'
              << "isnan(Inf) = " << std::isnan(INFINITY) << '\n'
              << "isnan(0.0) = " << std::isnan(0.0) << '\n'
              << "isnan(DBL_MIN/2.0) = " << std::isnan(DBL_MIN / 2.0) << '\n'
              << "isnan(0.0 / 0.0)   = " << std::isnan(0.0 / 0.0) << '\n'
              << "isnan(Inf - Inf)   = " << std::isnan(INFINITY - INFINITY) << '\n';
}
```

Output:

```text
isnan(NaN) = true
isnan(Inf) = false
isnan(0.0) = false
isnan(DBL_MIN/2.0) = false
isnan(0.0 / 0.0)   = true
isnan(Inf - Inf)   = true
```

### See also

- **nannanfnanl (C++11)(C++11)(C++11)** — not-a-number (NaN) (function)
- **fpclassify (C++11)** — categorizes the given floating-point value (function)
- **isfinite (C++11)** — checks if the given number has finite value (function)
- **isinf (C++11)** — checks if the given number is infinite (function)
- **isnormal (C++11)** — checks if the given number is normal (function)
- **isunordered (C++11)** — checks if two floating-point values are unordered
  (function)

**C documentation for `isnan`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/isnan*
