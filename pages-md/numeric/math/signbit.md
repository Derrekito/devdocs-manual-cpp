# std::signbit

```cpp
bool signbit( float num );
bool signbit( double num );
bool signbit( long double num );  // (since C++11) (until C++23)
constexpr bool signbit( /* floating-point-type */ num );  // (since C++23)
Additional overloads
template< class Integer >
bool signbit( Integer num );  // (A) (since C++11) (constexpr since C++23)
```

1) Determines if the given floating point number `num` is negative. The library
   provides overloads for all cv-unqualified floating-point types as the type of
   the parameter `num`.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
   `double`.

### Parameters

- **num** — floating-point or integer value

### Return value

`true` if `num` is negative, `false` otherwise.

### Notes

This function detects the sign bit of zeroes, infinities, and NaNs. Along with
`std::copysign`, `std::signbit` is one of the only two portable ways to examine
the sign of a NaN.

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their argument `num` of integer
type, `std::signbit(num)` has the same effect as
`std::signbit(static_cast<double>(num))`.

### Example

```cpp
#include <cmath>
#include <iostream>

int main()
{
    std::cout << std::boolalpha
              << "signbit(+0.0) = " << std::signbit(+0.0) << '\n'
              << "signbit(-0.0) = " << std::signbit(-0.0) << '\n'
              << "signbit(+nan) = " << std::signbit(+NAN) << '\n'
              << "signbit(-nan) = " << std::signbit(-NAN) << '\n'
              << "signbit(+inf) = " << std::signbit(+INFINITY) << '\n'
              << "signbit(-inf) = " << std::signbit(-INFINITY) << '\n';
}
```

Output:

```text
signbit(+0.0) = false
signbit(-0.0) = true
signbit(+nan) = false
signbit(-nan) = true
signbit(+inf) = false
signbit(-inf) = true
```

### See also

- **abs(float)fabsfabsffabsl (C++11)(C++11)** — absolute value of a floating
  point value (\(\small{|x|}\)|x|) (function)
- **copysigncopysignfcopysignl (C++11)(C++11)(C++11)** — copies the sign of a
  floating point value (function)

**C documentation for `signbit`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/signbit*
