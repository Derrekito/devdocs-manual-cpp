# std::riemann_zeta, std::riemann_zetaf, std::riemann_zetal

```cpp
float       riemann_zeta ( float num );
double      riemann_zeta ( double num );
long double riemann_zeta ( long double num );  // (since C++17) (until C++23)
/* floating-point-type */ riemann_zeta( /* floating-point-type */ num );  // (since C++23)
float       riemann_zetaf( float num );  // (2) (since C++17)
long double riemann_zetal( long double num );  // (3) (since C++17)
Additional overloads
template< class Integer >
double      riemann_zeta ( Integer num );  // (A) (since C++17)
```

1-3) Computes the Riemann zeta function of `num`. The library provides overloads
   of `std::riemann_zeta` for all cv-unqualified floating-point types as the
   type of the parameter `num`.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
   `double`.

### Parameters

- **num** — floating-point or value

### Return value

If no errors occur, value of the Riemann zeta function of `num`, ζ(num), defined
for the entire real axis:

- For num>1, Σ∞n=1n-num
- For 0≤num≤1, 1/21-num-1Σ∞n=1 (-1)n n-num
- For num<0, 2numπnum-1sin(πnum/2)Γ(1−num)ζ(1−num)

### Error handling

Errors may be reported as specified in `math_errhandling`.

- If the argument is NaN, NaN is returned and domain error is not reported

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
type, `std::riemann_zeta(num)` has the same effect as
`std::riemann_zeta(static_cast<double>(num))`.

### Example

```cpp
#include <cmath>
#include <format>
#include <iostream>
#include <numbers>

int main()
{
    constexpr auto π = std::numbers::pi;

    // spot checks for well-known values
    for (const double x : {-1.0, 0.0, 1.0, 0.5, 2.0})
        std::cout << std::format("ζ({})\t= {:+.5f}\n", x, std::riemann_zeta(x));
    std::cout << std::format("π²/6\t= {:+.5f}\n", π * π / 6);
}
```

Output:

```text
ζ(-1)   = -0.08333
ζ(0)    = -0.50000
ζ(1)    = +inf
ζ(0.5)  = -1.46035
ζ(2)    = +1.64493
π²/6    = +1.64493
```

### External links

  Weisstein, Eric W. "Riemann Zeta Function." From MathWorld — A Wolfram Web
      Resource.

---
*Source: https://en.cppreference.com/w/cpp/numeric/special_functions/riemann_zeta*
