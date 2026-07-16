# std::comp_ellint_2, std::comp_ellint_2f, std::comp_ellint_2l

```cpp
float       comp_ellint_2 ( float k );
double      comp_ellint_2 ( double k );
long double comp_ellint_2 ( long double k );  // (since C++17) (until C++23)
/* floating-point-type */ comp_ellint_2( /* floating-point-type */ k );  // (since C++23)
float       comp_ellint_2f( float k );  // (2) (since C++17)
long double comp_ellint_2l( long double k );  // (3) (since C++17)
Additional overloads
template< class Integer >
double      comp_ellint_2 ( Integer k );  // (A) (since C++17)
```

1-3) Computes the complete elliptic integral of the second kind of `k`. The
   library provides overloads of `std::comp_ellint_2` for all cv-unqualified
   floating-point types as the type of the parameter `k`.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
   `double`.

### Parameters

- **k** — elliptic modulus or eccentricity (a floating-point or integer value)

### Return value

If no errors occur, value of the complete elliptic integral of the second kind
of `k`, that is `std::ellint_2(k, π/2)`, is returned.

### Error handling

Errors may be reported as specified in `math_errhandling`.

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

The perimeter of an ellipse with eccentricity `k` and semimajor axis a equals
4aE(k), where E is `std::comp_ellint_2`. When eccentricity equals 0, the ellipse
degenerates to a circle with radius a and the perimeter equals 2πa, so E(0) =
π/2. When eccentricity equals 1, the ellipse degenerates to a line of length 2a,
whose perimeter is 4a, so E(1) = 1.

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their argument `num` of integer
type, `std::comp_ellint_2(num)` has the same effect as
`std::comp_ellint_2(static_cast<double>(num))`.

### Example

```cpp
#include <cmath>
#include <iostream>
#include <numbers>

int main()
{
    constexpr double hpi = std::numbers::pi / 2.0;

    std::cout << "E(0) = " << std::comp_ellint_2(0) << '\n'
              << "π/2 = " << hpi << '\n'
              << "E(1) = " << std::comp_ellint_2(1) << '\n'
              << "E(1, π/2) = " << std::ellint_2(1, hpi) << '\n';
}
```

Output:

```text
E(0) = 1.5708
π/2 = 1.5708
E(1) = 1
E(1, π/2) = 1
```

### See also

- **ellint_2ellint_2fellint_2l (C++17)(C++17)(C++17)** — (incomplete) elliptic
  integral of the second kind (function)

### External links

  Weisstein, Eric W. "Complete Elliptic Integral of the Second Kind." From
      MathWorld — A Wolfram Web Resource.

---
*Source: https://en.cppreference.com/w/cpp/numeric/special_functions/comp_ellint_2*
