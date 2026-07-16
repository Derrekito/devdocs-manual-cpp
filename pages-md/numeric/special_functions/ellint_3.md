# std::ellint_3, std::ellint_3f, std::ellint_3l

```cpp
float       ellint_3 ( float k, float nu, float phi );
double      ellint_3 ( double k, double nu, double phi );
long double ellint_3 ( long double k, long double nu, long double phi );  // (since C++17) (until C++23)
/* floating-point-type */ ellint_3( /* floating-point-type */ k,
                                    /* floating-point-type */ nu,
                                    /* floating-point-type */ phi );  // (since C++23)
float       ellint_3f( float k, float nu, float phi );  // (2) (since C++17)
long double ellint_3l( long double k, long double nu, long double phi );  // (3) (since C++17)
Additional overloads
template< class Arithmetic1, class Arithmetic2, class Arithmetic3 >
/* common-floating-point-type */
    ellint_3( Arithmetic1 k, Arithmetic2 nu, Arithmetic3 phi );  // (A) (since C++17)
```

1-3) Computes the incomplete elliptic integral of the third kind of `k`, `nu`,
   and `phi`. The library provides overloads of `std::ellint_3` for all
   cv-unqualified floating-point types as the type of the parameters `k`, `nu`
   and `phi`.(since C++23)

A) Additional overloads are provided for all other combinations of arithmetic
   types.

### Parameters

- **k** — elliptic modulus or eccentricity (a floating-point or integer value)
- **nu** — elliptic characteristic (a floating-point or integer value)
- **phi** — Jacobi amplitude (a floating-point or integer value, measured in
  radians)

### Return value

If no errors occur, value of the incomplete elliptic integral of the third kind
of `k`, `nu`, and `phi`, that is ∫phi0dθ/(1-nusin2θ)√1-k2sin2θ, is returned.

### Error handling

Errors may be reported as specified in `math_errhandling`:

- If the argument is NaN, NaN is returned and domain error is not reported.
- If |k|>1, a domain error may occur.

### Notes

Implementations that do not support C++17, but support ISO 29124:2010, provide
this function if `__STDCPP_MATH_SPEC_FUNCS__` is defined by the implementation
to a value at least 201003L and if the user defines
`__STDCPP_WANT_MATH_SPEC_FUNCS__` before including any standard library headers.

Implementations that do not support ISO 29124:2010 but support TR 19768:2007
(TR1), provide this function in the header `tr1/cmath` and namespace `std::tr1`.

An implementation of this function is also available in boost.math.

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their first argument `num1`,
second argument `num2` and third argument `num3`:

- If `num1`, `num2` or `num3` has type long double, then `std::ellint_3(num1,
  num2, num3)` has the same effect as `std::ellint_3(static_cast<long
  double>(num1), static_cast<long double>(num2), static_cast<long
  double>(num3))`.
- Otherwise, if `num1`, `num2` and/or `num3` has type double or an integer type,
  then `std::ellint_3(num1, num2, num3)` has the same effect as
  `std::ellint_3(static_cast<double>(num1), static_cast<double>(num2),
  static_cast<double>(num3))`.
- Otherwise, if `num1`, `num2` or `num3` has type float, then
  `std::ellint_3(num1, num2, num3)` has the same effect as
  `std::ellint_3(static_cast<float>(num1), static_cast<float>(num2),
  static_cast<float>(num3))`.
*(until C++23)*

If `num1`, `num2` and `num3` have arithmetic types, then `std::ellint_3(num1,
num2, num3)` has the same effect as `std::ellint_3(static_cast</*
common-floating-point-type */>(num1), static_cast</* common-floating-point-type
*/>(num2), static_cast</* common-floating-point-type */>(num3))`, where /*
common-floating-point-type */ is the floating-point type with the greatest
floating-point conversion rank and greatest floating-point conversion subrank
among the types of `num1`, `num2` and `num3`, arguments of integer type are
considered to have the same floating-point conversion rank as double.
If no such floating-point type with the greatest rank and subrank exists, then
overload resolution does not result in a usable candidate from the overloads
provided.
*(since C++23)*

### Example

```cpp
#include <cmath>
#include <iostream>
#include <numbers>

int main()
{
    const double hpi = std::numbers::pi / 2;

    std::cout << "Π(0,0,π/2) = " << std::ellint_3(0, 0, hpi) << '\n'
              << "π/2 = " << hpi << '\n';
}
```

Output:

```text
Π(0,0,π/2) = 1.5708
π/2 = 1.5708
```

### See also

- **comp_ellint_3comp_ellint_3fcomp_ellint_3l (C++17)(C++17)(C++17)** —
  (complete) elliptic integral of the third kind (function)

### External links

  Weisstein, Eric W. "Elliptic Integral of the Third Kind." From MathWorld — A
      Wolfram Web Resource.

---
*Source: https://en.cppreference.com/w/cpp/numeric/special_functions/ellint_3*
