# std::islessequal

```cpp
bool islessequal( float x, float y );
bool islessequal( double x, double y );
bool islessequal( long double x, long double y );  // (since C++11) (until C++23)
constexpr bool islessequal( /* floating-point-type */ x,
                            /* floating-point-type */ y );  // (since C++23)
Additional overloads
template< class Arithmetic1, class Arithmetic2 >
bool islessequal( Arithmetic1 x, Arithmetic2 y );  // (A) (since C++11) (constexpr since C++23)
```

1) Determines if the floating point number `x` is less than or equal to the
   floating-point number `y`, without setting floating-point exceptions. The
   library provides overloads for all cv-unqualified floating-point types as the
   type of the parameters `x` and `y`.(since C++23)

A) Additional overloads are provided for all other combinations of arithmetic
   types.

### Parameters

- **x, y** — floating-point or integer values

### Return value

`true` if `x <= y`, `false` otherwise.

### Notes

The built-in operator<= for floating-point numbers may raise `FE_INVALID` if one
or both of the arguments is NaN. This function is a "quiet" version of
operator<=.

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their first argument `num1` and
second argument `num2`:

- If `num1` or `num2` has type long double, then `std::islessequal(num1, num2)`
  has the same effect as `std::islessequal(static_cast<long double>(num1),
  static_cast<long double>(num2))`.
- Otherwise, if `num1` and/or `num2` has type double or an integer type, then
  `std::islessequal(num1, num2)` has the same effect as
  `std::islessequal(static_cast<double>(num1), static_cast<double>(num2))`.
- Otherwise, if `num1` or `num2` has type float, then `std::islessequal(num1,
  num2)` has the same effect as `std::islessequal(static_cast<float>(num1),
  static_cast<float>(num2))`.
*(until C++23)*

If `num1` and `num2` have arithmetic types, then `std::islessequal(num1, num2)`
has the same effect as `std::islessequal(static_cast</*
common-floating-point-type */>(num1), static_cast</* common-floating-point-type
*/>(num2))`, where /* common-floating-point-type */ is the floating-point type
with the greatest floating-point conversion rank and greatest floating-point
conversion subrank between the types of `num1` and `num2`, arguments of integer
type are considered to have the same floating-point conversion rank as double.
If no such floating-point type with the greatest rank and subrank exists, then
overload resolution does not result in a usable candidate from the overloads
provided.
*(since C++23)*

### See also

- **less_equal** — function object implementing `x <= y` (class template)
- **isgreaterequal (C++11)** — checks if the first floating-point argument is
  greater or equal than the second (function)

**C documentation for `islessequal`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/islessequal*
