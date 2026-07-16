# std::fmin, std::fminf, std::fminl

```cpp
float       fmin ( float x, float y );
double      fmin ( double x, double y );
long double fmin ( long double x, long double y );  // (until C++23)
constexpr /* floating-point-type */
            fmin ( /* floating-point-type */ x,
                   /* floating-point-type */ y );  // (since C++23)
float       fminf( float x, float y );  // (2) (since C++11) (constexpr since C++23)
long double fminl( long double x, long double y );  // (3) (since C++11) (constexpr since C++23)
Additional overloads (since C++11)
template< class Integer >
double      fmin ( Integer x, Integer y );  // (A) (constexpr since C++23)
```

1-3) Returns the smaller of two floating point arguments, treating NaNs as
   missing data (between a NaN and a numeric value, the numeric value is
   chosen). The library provides overloads of `std::fmin` for all cv-unqualified
   floating-point types as the type of the parameters.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
double.
*(since C++11)*

### Parameters

- **x, y** — floating-point or integer values

### Return value

If successful, returns the smaller of two floating point values. The value
returned is exact and does not depend on any rounding modes.

### Error handling

This function is not subject to any of the error conditions specified in
`math_errhandling`.

If the implementation supports IEEE floating-point arithmetic (IEC 60559),

- If one of the two arguments is NaN, the value of the other argument is
  returned.
- Only if both arguments are NaN, NaN is returned.

### Notes

This function is not required to be sensitive to the sign of zero, although some
implementations additionally enforce that if one argument is `+0` and the other
is `-0`, then `-0` is returned.

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their first argument `num1` and
second argument `num2`:

- If `num1` or `num2` has type long double, then `std::fmin(num1, num2)` has the
  same effect as `std::fmin(static_cast<long double>(num1), static_cast<long
  double>(num2))`.
- Otherwise, if `num1` and/or `num2` has type double or an integer type, then
  `std::fmin(num1, num2)` has the same effect as
  `std::fmin(static_cast<double>(num1), static_cast<double>(num2))`.
- Otherwise, if `num1` or `num2` has type float, then `std::fmin(num1, num2)`
  has the same effect as `std::fmin(static_cast<float>(num1),
  static_cast<float>(num2))`.
*(until C++23)*

If `num1` and `num2` have arithmetic types, then `std::fmin(num1, num2)` has the
same effect as `std::fmin(static_cast</* common-floating-point-type */>(num1),
static_cast</* common-floating-point-type */>(num2))`, where /*
common-floating-point-type */ is the floating-point type with the greatest
floating-point conversion rank and greatest floating-point conversion subrank
between the types of `num1` and `num2`, arguments of integer type are considered
to have the same floating-point conversion rank as double.
If no such floating-point type with the greatest rank and subrank exists, then
overload resolution does not result in a usable candidate from the overloads
provided.
*(since C++23)*

### Example

```cpp
#include <cmath>
#include <iostream>

int main()
{
    std::cout << "fmin(2,1)    = " << std::fmin(2, 1) << '\n'
              << "fmin(-Inf,0) = " << std::fmin(-INFINITY, 0) << '\n'
              << "fmin(NaN,-1) = " << std::fmin(NAN, -1) << '\n';
}
```

Possible output:

```text
fmin(2,1)    = 1
fmin(-Inf,0) = -inf
fmin(NaN,-1) = -1
```

### See also

- **isless (C++11)** — checks if the first floating-point argument is less than
  the second (function)
- **fmaxfmaxffmaxl (C++11)(C++11)(C++11)** — larger of two floating-point values
  (function)
- **min** — returns the smaller of the given values (function template)
- **min_element** — returns the smallest element in a range (function template)
- **minmax (C++11)** — returns the smaller and larger of two elements (function
  template)
- **minmax_element (C++11)** — returns the smallest and the largest elements in
  a range (function template)

**C documentation for `fmin`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/fmin*
