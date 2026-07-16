# std::comp_ellint_3, std::comp_ellint_3f, std::comp_ellint_3l

```cpp
float       comp_ellint_3 ( float k, float nu );
double      comp_ellint_3 ( double k, double nu );
long double comp_ellint_3 ( long double k, long double nu );  // (since C++17) (until C++23)
/* floating-point-type */ comp_ellint_3( /* floating-point-type */ k,
                                         /* floating-point-type */ nu );  // (since C++23)
float       comp_ellint_3f( float k, float nu );  // (2) (since C++17)
long double comp_ellint_3l( long double k, long double nu );  // (3) (since C++17)
Additional overloads
template< class Arithmetic1, class Arithmetic2 >
/* common-floating-point-type */
    comp_ellint_3( Arithmetic1 k, Arithmetic2 nu );  // (A) (since C++17)
```

1-3) Computes the complete elliptic integral of the third kind of the arguments
   `k` and `nu`. The library provides overloads of `std::comp_ellint_3` for all
   cv-unqualified floating-point types as the type of the parameters `k` and
   `nu`.(since C++23)

A) Additional overloads are provided for all other combinations of arithmetic
   types.

### Parameters

- **k** — elliptic modulus or eccentricity (a floating-point or integer value)
- **nu** — elliptic characteristic (a floating-point or integer value)

### Return value

If no errors occur, value of the complete elliptic integral of the third kind of
`k` and `nu`, that is `std::ellint_3(k, nu, π/2)`, is returned.

### Error handling

Errors may be reported as specified in `math_errhandling`.

- If the argument is NaN, NaN is returned and domain error is not reported
- If |k|>1, a domain error may occur

### Notes

Implementations that do not support C++17, but support ISO 29124:2010, provide
this function if `__STDCPP_MATH_SPEC_FUNCS__` is defined by the implementation
to a value at least 201003L and if the user defines
`__STDCPP_WANT_MATH_SPEC_FUNCS__` before including any standard library headers.

Implementations that do not support ISO 29124:2010 but support TR 19768:2007
(TR1), provide this function in the header `tr1/cmath` and namespace `std::tr1`.

An implementation of this function is also available in boost.math.

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their first argument `num1` and
second argument `num2`:

- If `num1` or `num2` has type long double, then `std::comp_ellint_3(num1,
  num2)` has the same effect as `std::comp_ellint_3(static_cast<long
  double>(num1), static_cast<long double>(num2))`.
- Otherwise, if `num1` and/or `num2` has type double or an integer type, then
  `std::comp_ellint_3(num1, num2)` has the same effect as
  `std::comp_ellint_3(static_cast<double>(num1), static_cast<double>(num2))`.
- Otherwise, if `num1` or `num2` has type float, then `std::comp_ellint_3(num1,
  num2)` has the same effect as `std::comp_ellint_3(static_cast<float>(num1),
  static_cast<float>(num2))`.
*(until C++23)*

If `num1` and `num2` have arithmetic types, then `std::comp_ellint_3(num1,
num2)` has the same effect as `std::comp_ellint_3(static_cast</*
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

int main()
{
    std::cout << std::fixed
              << "Π(0.5,0) = " << std::comp_ellint_3(0.5, 0) << '\n'
              << "K(0.5)   = " << std::comp_ellint_1(0.5) << '\n'
              << "Π(0,0)   = " << std::comp_ellint_3(0, 0) << '\n'
              << "π/2      = " << std::acos(-1) / 2 << '\n'
              << "Π(0.5,1) = " << std::comp_ellint_3(0.5, 1) << '\n';
}
```

Output:

```text
Π(0.5,0) = 1.685750
K(0.5)   = 1.685750
Π(0,0)   = 1.570796
π/2      = 1.570796
Π(0.5,1) = inf
```

### See also

- **ellint_3ellint_3fellint_3l (C++17)(C++17)(C++17)** — (incomplete) elliptic
  integral of the third kind (function)

### External links

  Weisstein, Eric W. "Elliptic Integral of the Third Kind." From MathWorld — A
      Wolfram Web Resource.

---
*Source: https://en.cppreference.com/w/cpp/numeric/special_functions/comp_ellint_3*
