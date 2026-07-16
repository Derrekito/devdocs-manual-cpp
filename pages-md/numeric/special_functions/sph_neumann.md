# std::sph_neumann, std::sph_neumannf, std::sph_neumannl

```cpp
float       sph_neumann ( unsigned n, float x );
double      sph_neumann ( unsigned n, double x );
long double sph_neumann ( unsigned n, long double x );  // (since C++17) (until C++23)
/* floating-point-type */ sph_neumann( unsigned n,
                                       /* floating-point-type */ x );  // (since C++23)
float       sph_neumannf( unsigned n, float x );  // (2) (since C++17)
long double sph_neumannl( unsigned n, long double x );  // (3) (since C++17)
Additional overloads
template< class Integer >
double      sph_neumann ( unsigned n, Integer x );  // (A) (since C++17)
```

1-3) Computes the spherical Bessel function of the second kind, also known as
   the spherical Neumann function, of `n` and `x`. The library provides
   overloads of `std::sph_neumann` for all cv-unqualified floating-point types
   as the type of the parameter `x`.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
   `double`.

### Parameters

- **n** — the order of the function
- **x** — the argument of the function

### Return value

If no errors occur, returns the value of the spherical Bessel function of the
second kind (spherical Neumann function) of `n` and `x`, that is nn(x) =
(π/2x)1/2Nn+1/2(x) where Nn(x) is `std::cyl_neumann(n, x)` and x≥0.

### Error handling

Errors may be reported as specified in `math_errhandling`

- If the argument is NaN, NaN is returned and domain error is not reported
- If n≥128, the behavior is implementation-defined

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
type, `std::sph_neumann(int_num, num)` has the same effect as
`std::sph_neumann(int_num, static_cast<double>(num))`.

### Example

```cpp
#include <cmath>
#include <iostream>

int main()
{
    // spot check for n == 1
    double x = 1.2345;
    std::cout << "n_1(" << x << ") = " << std::sph_neumann(1, x) << '\n';

    // exact solution for n_1
    std::cout << "-cos(x)/x² - sin(x)/x = "
              << -std::cos(x) / (x * x) - std::sin(x) / x << '\n';
}
```

Output:

```text
n_1(1.2345) = -0.981201
-cos(x)/x² - sin(x)/x = -0.981201
```

### See also

- **cyl_neumanncyl_neumannfcyl_neumannl (C++17)(C++17)(C++17)** — cylindrical
  Neumann functions (function)
- **sph_besselsph_besselfsph_bessell (C++17)(C++17)(C++17)** — spherical Bessel
  functions (of the first kind) (function)

### External links

  Weisstein, Eric W. "Spherical Bessel Function of the Second Kind." From
      MathWorld — A Wolfram Web Resource.

---
*Source: https://en.cppreference.com/w/cpp/numeric/special_functions/sph_neumann*
