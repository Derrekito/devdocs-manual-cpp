# std::sph_legendre, std::sph_legendref, std::sph_legendrel

```cpp
float       sph_legendre ( unsigned l, unsigned m, float theta );
double      sph_legendre ( unsigned l, unsigned m, double theta );
long double sph_legendre ( unsigned l, unsigned m, long double theta );  // (since C++17) (until C++23)
/* floating-point-type */ sph_legendre( unsigned l, unsigned m,
                                        /* floating-point-type */ theta );  // (since C++23)
float       sph_legendref( unsigned l, unsigned m, float theta );  // (2) (since C++17)
long double sph_legendrel( unsigned l, unsigned m, long double theta );  // (3) (since C++17)
Additional overloads
template< class Integer >
double      sph_legendre ( unsigned l, unsigned m, Integer theta );  // (A) (since C++17)
```

1-3) Computes the spherical associated Legendre function of degree `l`, order
   `m`, and polar angle `theta`. The library provides overloads of
   `std::sph_legendre` for all cv-unqualified floating-point types as the type
   of the parameter `theta`.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
   `double`.

### Parameters

- **l** — degree
- **m** — order
- **theta** — polar angle, measured in radians

### Return value

If no errors occur, returns the value of the spherical associated Legendre
function (that is, spherical harmonic with ϕ = 0) of `l`, `m`, and `theta`,
where the spherical harmonic function is defined as Yml(theta,ϕ) =
(-1)m[(2l+1)(l-m)!/4π(l+m)!]1/2Pml(cos(theta))eimϕ where Pml(x) is
`std::assoc_legendre(l, m, x)`) and |m|≤l.

Note that the Condon-Shortley phase term (-1)m is included in this definition
because it is omitted from the definition of Pml in `std::assoc_legendre`.

### Error handling

Errors may be reported as specified in `math_errhandling`.

- If the argument is NaN, NaN is returned and domain error is not reported.
- If l≥128, the behavior is implementation-defined.

### Notes

Implementations that do not support C++17, but support ISO 29124:2010, provide
this function if `__STDCPP_MATH_SPEC_FUNCS__` is defined by the implementation
to a value at least 201003L and if the user defines
`__STDCPP_WANT_MATH_SPEC_FUNCS__` before including any standard library headers.

Implementations that do not support ISO 29124:2010 but support TR 19768:2007
(TR1), provide this function in the header `tr1/cmath` and namespace `std::tr1`.

An implementation of the spherical harmonic function is available in boost.math,
and it reduces to this function when called with the parameter phi set to zero.

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their argument `num` of integer
type, `std::sph_legendre(int_num1, int_num2, num)` has the same effect as
`std::sph_legendre(int_num1, int_num2, static_cast<double>(num))`.

### Example

```cpp
#include <cmath>
#include <iostream>
#include <numbers>

int main()
{
    // spot check for l=3, m=0
    double x = 1.2345;
    std::cout << "Y_3^0(" << x << ") = " << std::sph_legendre(3, 0, x) << '\n';

    // exact solution
    std::cout << "exact solution = "
              << 0.25 * std::sqrt(7 / std::numbers::pi)
                  * (5 * std::pow(std::cos(x), 3) - 3 * std::cos(x))
              << '\n';
}
```

Output:

```text
Y_3^0(1.2345) = -0.302387
exact solution = -0.302387
```

### See also

- **assoc_legendreassoc_legendrefassoc_legendrel (C++17)(C++17)(C++17)** —
  associated Legendre polynomials (function)

### External links

  Weisstein, Eric W. "Spherical Harmonic." From MathWorld — A Wolfram Web
      Resource.

---
*Source: https://en.cppreference.com/w/cpp/numeric/special_functions/sph_legendre*
