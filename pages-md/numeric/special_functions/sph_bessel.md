# std::sph_bessel, std::sph_besself, std::sph_bessell

```cpp
float       sph_bessel ( unsigned int n, float x );
double      sph_bessel ( unsigned int n, double x );
long double sph_bessel ( unsigned int n, long double x );  // (since C++17) (until C++23)
/* floating-point-type */ sph_bessel( unsigned int n,
                                      /* floating-point-type */ x );  // (since C++23)
float       sph_besself( unsigned int n, float x );  // (2) (since C++17)
long double sph_bessell( unsigned int n, long double x );  // (3) (since C++17)
Additional overloads
template< class Integer >
double      sph_bessel ( unsigned int n, Integer x );  // (A) (since C++17)
```

1-3) Computes the spherical Bessel function of the first kind of `n` and `x`.
   The library provides overloads of `std::sph_bessel` for all cv-unqualified
   floating-point types as the type of the parameter `x`.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
   `double`.

### Parameters

- **n** — the order of the function
- **x** — the argument of the function

### Return value

If no errors occur, returns the value of the spherical Bessel function of the
first kind of `n` and `x`, that is jn(x) = (π/2x)1/2Jn+1/2(x) where Jn(x) is
`std::cyl_bessel_j(n, x)` and x≥0.

### Error handling

Errors may be reported as specified in `math_errhandling`.

- If the argument is NaN, NaN is returned and domain error is not reported.
- If n≥128, the behavior is implementation-defined.

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
type, `std::sph_bessel(int_num, num)` has the same effect as
`std::sph_bessel(int_num, static_cast<double>(num))`.

### Example

```cpp
#include <cmath>
#include <iostream>

int main()
{
    // spot check for n == 1
    double x = 1.2345;
    std::cout << "j_1(" << x << ") = " << std::sph_bessel(1, x) << '\n';

    // exact solution for j_1
    std::cout << "sin(x)/x² - cos(x)/x = "
              << std::sin(x) / (x * x) - std::cos(x) / x << '\n';
}
```

Output:

```text
j_1(1.2345) = 0.352106
sin(x)/x² - cos(x)/x = 0.352106
```

### See also

- **cyl_bessel_jcyl_bessel_jfcyl_bessel_jl (C++17)(C++17)(C++17)** — cylindrical
  Bessel functions (of the first kind) (function)
- **sph_neumannsph_neumannfsph_neumannl (C++17)(C++17)(C++17)** — spherical
  Neumann functions (function)

### External links

  Weisstein, Eric W. "Spherical Bessel Function of the First Kind." From
      MathWorld — A Wolfram Web Resource.

---
*Source: https://en.cppreference.com/w/cpp/numeric/special_functions/sph_bessel*
