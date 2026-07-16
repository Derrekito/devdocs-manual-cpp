# std::hermite, std::hermitef, std::hermitel

```cpp
double      hermite ( unsigned int n, double x );
float       hermite ( unsigned int n, float x );
long double hermite ( unsigned int n, long double x );  // (since C++17) (until C++23)
/* floating-point-type */ hermite( unsigned int n,
                                   /* floating-point-type */ x );  // (since C++23)
float       hermitef( unsigned int n, float x );  // (2) (since C++17)
long double hermitel( unsigned int n, long double x );  // (3) (since C++17)
Additional overloads
template< class Integer >
double      hermite ( unsigned int n, Integer x );  // (A) (since C++17)
```

1-3) Computes the (physicist's) Hermite polynomials of the degree `n` and
   argument `x`. The library provides overloads of `std::hermite` for all
   cv-unqualified floating-point types as the type of the parameter `x`.(since
   C++23)

A) Additional overloads are provided for all integer types, which are treated as
   `double`.

### Parameters

- **n** — the degree of the polynomial
- **x** — the argument, a floating-point or integer value

### Return value

If no errors occur, value of the order-`n` Hermite polynomial of `x`, that is
(-1)n*e*x2dn/dxn*e*-x2, is returned.

### Error handling

Errors may be reported as specified in `math_errhandling`.

- If the argument is NaN, NaN is returned and domain error is not reported.
- If `n` is greater or equal than 128, the behavior is implementation-defined.

### Notes

Implementations that do not support C++17, but support ISO 29124:2010, provide
this function if `__STDCPP_MATH_SPEC_FUNCS__` is defined by the implementation
to a value at least 201003L and if the user defines
`__STDCPP_WANT_MATH_SPEC_FUNCS__` before including any standard library headers.

Implementations that do not support ISO 29124:2010 but support TR 19768:2007
(TR1), provide this function in the header `tr1/cmath` and namespace `std::tr1`.

An implementation of this function is also available in boost.math.

The Hermite polynomials are the polynomial solutions of the equation u,,-2xu, =
-2nu.

The first few are:

  Function | Polynomial
  hermite(0, x) | 1
  hermite(1, x) | 2x
  hermite(2, x) | 4x2 - 2
  hermite(3, x) | 8x3 - 12x
  hermite(4, x) | 16x4 - 48x2 + 12

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their argument `num` of integer
type, `std::hermite(int_num, num)` has the same effect as `std::hermite(int_num,
static_cast<double>(num))`.

### Example

```cpp
#include <cmath>
#include <iostream>

double H3(double x)
{
    return 8 * std::pow(x, 3) - 12 * x;
}

double H4(double x)
{
    return 16 * std::pow(x, 4) - 48 * x * x + 12;
}

int main()
{
    // spot-checks
    std::cout << std::hermite(3, 10) << '=' << H3(10) << '\n'
              << std::hermite(4, 10) << '=' << H4(10) << '\n';
}
```

Output:

```text
7880=7880
155212=155212
```

### See also

- **laguerrelaguerreflaguerrel (C++17)(C++17)(C++17)** — Laguerre polynomials
  (function)
- **legendrelegendreflegendrel (C++17)(C++17)(C++17)** — Legendre polynomials
  (function)

### External links

  Weisstein, Eric W. "Hermite Polynomial." From MathWorld — A Wolfram Web
      Resource.

---
*Source: https://en.cppreference.com/w/cpp/numeric/special_functions/hermite*
