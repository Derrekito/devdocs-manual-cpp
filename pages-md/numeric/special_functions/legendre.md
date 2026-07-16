# std::legendre, std::legendref, std::legendrel

```cpp
float       legendre ( unsigned int n, float x );
double      legendre ( unsigned int n, double x );
long double legendre ( unsigned int n, long double x );  // (since C++17) (until C++23)
/* floating-point-type */ legendre( unsigned int n,
                                    /* floating-point-type */ x );  // (since C++23)
float       legendref( unsigned int n, float x );  // (2) (since C++17)
long double legendrel( unsigned int n, long double x );  // (3) (since C++17)
Additional overloads
template< class Integer >
double      legendre ( unsigned int n, Integer x );  // (A) (since C++17)
```

1-3) Computes the unassociated Legendre polynomials of the degree `n` and
   argument `x`. The library provides overloads of `std::legendre` for all
   cv-unqualified floating-point types as the type of the parameter `x`.(since
   C++23)

A) Additional overloads are provided for all integer types, which are treated as
   `double`.

### Parameters

- **n** — the degree of the polynomial
- **x** — the argument, a floating-point or integer value

### Return value

If no errors occur, value of the order-`n` unassociated Legendre polynomial of
`x`, that is \(\mathsf{P}_n(x) = \frac{1}{2^n n!}
\frac{\mathsf{d}^n}{\mathsf{d}x^n} (x^2-1)^n \)1/2nn!dn/dxn(x2-1)n, is returned.

### Error handling

Errors may be reported as specified in `math_errhandling`.

- If the argument is NaN, NaN is returned and domain error is not reported
- The function is not required to be defined for |x|>1
- If `n` is greater or equal than 128, the behavior is implementation-defined

### Notes

Implementations that do not support C++17, but support ISO 29124:2010, provide
this function if `__STDCPP_MATH_SPEC_FUNCS__` is defined by the implementation
to a value at least 201003L and if the user defines
`__STDCPP_WANT_MATH_SPEC_FUNCS__` before including any standard library headers.

Implementations that do not support ISO 29124:2010 but support TR 19768:2007
(TR1), provide this function in the header `tr1/cmath` and namespace `std::tr1`.

An implementation of this function is also available in boost.math.

The first few Legendre polynomials are:

  Function | Polynomial
  legendre(0, x) | 1
  legendre(1, x) | x
  legendre(2, x) | 1/2(3x2 - 1)
  legendre(3, x) | 1/2(5x3 - 3x)
  legendre(4, x) | 1/8(35x4 - 30x2 + 3)

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their argument `num` of integer
type, `std::legendre(int_num, num)` has the same effect as
`std::legendre(int_num, static_cast<double>(num))`.

### Example

```cpp
#include <cmath>
#include <iostream>

double P3(double x)
{
    return 0.5 * (5 * std::pow(x, 3) - 3 * x);
}

double P4(double x)
{
    return 0.125 * (35 * std::pow(x, 4) - 30 * x * x + 3);
}

int main()
{
    // spot-checks
    std::cout << std::legendre(3, 0.25) << '=' << P3(0.25) << '\n'
              << std::legendre(4, 0.25) << '=' << P4(0.25) << '\n';
}
```

Output:

```text
-0.335938=-0.335938
0.157715=0.157715
```

### See also

- **laguerrelaguerreflaguerrel (C++17)(C++17)(C++17)** — Laguerre polynomials
  (function)
- **hermitehermitefhermitel (C++17)(C++17)(C++17)** — Hermite polynomials
  (function)

### External links

  Weisstein, Eric W. "Legendre Polynomial." From MathWorld — A Wolfram Web
      Resource.

---
*Source: https://en.cppreference.com/w/cpp/numeric/special_functions/legendre*
