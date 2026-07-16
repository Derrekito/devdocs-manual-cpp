# std::isunordered

```cpp
bool isunordered( float x, float y );
bool isunordered( double x, double y );
bool isunordered( long double x, long double y );  // (since C++11) (until C++23)
constexpr bool isunordered( /* floating-point-type */ x,
                            /* floating-point-type */ y );  // (since C++23)
Additional overloads
template< class Arithmetic1, class Arithmetic2 >
bool isunordered( Arithmetic1 x, Arithmetic2 y );  // (A) (since C++11) (constexpr since C++23)
```

1) Determines if the floating point numbers `x` and `y` are unordered, that is,
   one or both are NaN and thus cannot be meaningfully compared with each other.
   The library provides overloads for all cv-unqualified floating-point types as
   the type of the parameters `x` and `y`.(since C++23)

A) Additional overloads are provided for all other combinations of arithmetic
   types.

### Parameters

- **x, y** — floating-point or integer values

### Return value

`true` if either `x` or `y` is NaN, `false` otherwise.

### Notes

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their first argument `num1` and
second argument `num2`:

- If `num1` or `num2` has type long double, then `std::isunordered(num1, num2)`
  has the same effect as `std::isunordered(static_cast<long double>(num1),
  static_cast<long double>(num2))`.
- Otherwise, if `num1` and/or `num2` has type double or an integer type, then
  `std::isunordered(num1, num2)` has the same effect as
  `std::isunordered(static_cast<double>(num1), static_cast<double>(num2))`.
- Otherwise, if `num1` or `num2` has type float, then `std::isunordered(num1,
  num2)` has the same effect as `std::isunordered(static_cast<float>(num1),
  static_cast<float>(num2))`.
*(until C++23)*

If `num1` and `num2` have arithmetic types, then `std::isunordered(num1, num2)`
has the same effect as `std::isunordered(static_cast</*
common-floating-point-type */>(num1), static_cast</* common-floating-point-type
*/>(num2))`, where /* common-floating-point-type */ is the floating-point type
with the greatest floating-point conversion rank and greatest floating-point
conversion subrank between the types of `num1` and `num2`, arguments of integer
type are considered to have the same floating-point conversion rank as double.
If no such floating-point type with the greatest rank and subrank exists, then
overload resolution does not result in a usable candidate from the overloads
provided.
*(since C++23)*

### Example

```cpp
#include <cmath>
#include <iostream>

#define SHOW_UNORDERED(x, y) \
    std::cout << std::boolalpha << "isunordered(" \
              << #x << ", " << #y << "): " \
              << std::isunordered(x, y) << '\n'

int main()
{
    SHOW_UNORDERED(10, 01);
    SHOW_UNORDERED(INFINITY, NAN);
    SHOW_UNORDERED(INFINITY, INFINITY);
    SHOW_UNORDERED(NAN, NAN);
}
```

Output:

```text
isunordered(10, 01): false
isunordered(INFINITY, NAN): true
isunordered(INFINITY, INFINITY): false
isunordered(NAN, NAN): true
```

### See also

- **fpclassify (C++11)** — categorizes the given floating-point value (function)
- **isnan (C++11)** — checks if the given number is NaN (function)

**C documentation for `isunordered`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/isunordered*
