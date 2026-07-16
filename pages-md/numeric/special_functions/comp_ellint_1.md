# std::comp_ellint_1, std::comp_ellint_1f, std::comp_ellint_1l

```cpp
double      comp_ellint_1 ( double k );
float       comp_ellint_1 ( float k );
long double comp_ellint_1 ( long double k );  // (since C++17) (until C++23)
/* floating-point-type */ comp_ellint_1( /* floating-point-type */ k );  // (since C++23)
float       comp_ellint_1f( float k );  // (2) (since C++17)
long double comp_ellint_1l( long double k );  // (3) (since C++17)
Additional overloads
template< class Integer >
double      comp_ellint_1 ( Integer k );  // (A) (since C++17)
```

1-3) Computes the complete elliptic integral of the first kind of `k`. The
   library provides overloads of `std::comp_ellint_1` for all cv-unqualified
   floating-point types as the type of the parameter `k`.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
   `double`.

### Parameters

- **k** — elliptic modulus or eccentricity (a floating-point or integer value)

### Return value

If no errors occur, value of the complete elliptic integral of the first kind of
`k`, that is `std::ellint_1(k, π/2)`, is returned.

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

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their argument `num` of integer
type, `std::comp_ellint_1(num)` has the same effect as
`std::comp_ellint_1(static_cast<double>(num))`.

### Example

The period of a pendulum of length l, given acceleration due to gravity g, and
initial angle θ equals 4⋅√l/g⋅K(sin(θ/2)), where K is `std::comp_ellint_1`.

```cpp
#include <cmath>
#include <iostream>
#include <numbers>

int main()
{
    constexpr double π{std::numbers::pi};

    std::cout << "K(0) ≈ " << std::comp_ellint_1(0) << '\n'
              << "π/2 ≈ " << π / 2 << '\n'
              << "K(0.5) ≈ " << std::comp_ellint_1(0.5) << '\n'
              << "F(0.5, π/2) ≈ " << std::ellint_1(0.5, π / 2) << '\n'
              << "The period of a pendulum length 1m at 10° initial angle ≈ "
              << 4 * std::sqrt(1 / 9.80665) * std::comp_ellint_1(std::sin(π / 18 / 2))
              << "s,\n" "whereas the linear approximation gives ≈ "
              << 2 * π * std::sqrt(1 / 9.80665) << '\n';
}
```

Output:

```text
K(0) ≈ 1.5708
π/2 ≈ 1.5708
K(0.5) ≈ 1.68575
F(0.5, π/2) ≈ 1.68575
The period of a pendulum length 1 m at 10° initial angle ≈ 2.01024s,
whereas the linear approximation gives ≈ 2.00641
```

### See also

- **ellint_1ellint_1fellint_1l (C++17)(C++17)(C++17)** — (incomplete) elliptic
  integral of the first kind (function)

### External links

  Weisstein, Eric W. "Complete Elliptic Integral of the First Kind." From
      MathWorld — A Wolfram Web Resource.

---
*Source: https://en.cppreference.com/w/cpp/numeric/special_functions/comp_ellint_1*
