# std::asinh(std::complex)

```cpp
template< class T >
complex<T> asinh( const complex<T>& z );  // (since C++11)
```

Computes complex arc hyperbolic sine of a complex value `z` with branch cuts
outside the interval [−i; +i] along the imaginary axis.

### Parameters

- **z** — complex value

### Return value

If no errors occur, the complex arc hyperbolic sine of `z` is returned, in the
range of a strip mathematically unbounded along the real axis and in the
interval [−iπ/2; +iπ/2] along the imaginary axis.

### Error handling and special values

Errors are reported consistent with `math_errhandling`.

If the implementation supports IEEE floating-point arithmetic,

- `std::asinh(std::conj(z)) == std::conj(std::asinh(z))`
- `std::asinh(-z) == -std::asinh(z)`
- If `z` is `(+0,+0)`, the result is `(+0,+0)`
- If `z` is `(x,+∞)` (for any positive finite x), the result is `(+∞,π/2)`
- If `z` is `(x,NaN)` (for any finite x), the result is `(NaN,NaN)` and
  `FE_INVALID` may be raised
- If `z` is `(+∞,y)` (for any positive finite y), the result is `(+∞,+0)`
- If `z` is `(+∞,+∞)`, the result is `(+∞,π/4)`
- If `z` is `(+∞,NaN)`, the result is `(+∞,NaN)`
- If `z` is `(NaN,+0)`, the result is `(NaN,+0)`
- If `z` is `(NaN,y)` (for any finite nonzero y), the result is `(NaN,NaN)` and
  `FE_INVALID` may be raised
- If `z` is `(NaN,+∞)`, the result is `(±∞,NaN)` (the sign of the real part is
  unspecified)
- If `z` is `(NaN,NaN)`, the result is `(NaN,NaN)`

### Notes

Although the C++ standard names this function "complex arc hyperbolic sine", the
inverse functions of the hyperbolic functions are the area functions. Their
argument is the area of a hyperbolic sector, not an arc. The correct name is
"complex inverse hyperbolic sine", and, less common, "complex area hyperbolic
sine".

Inverse hyperbolic sine is a multivalued function and requires a branch cut on
the complex plane. The branch cut is conventionally placed at the line segments
(-*i*∞,-*i*) and (*i*,*i*∞) of the imaginary axis.

The mathematical definition of the principal value of the inverse hyperbolic
sine is asinh z = ln(z + √1+z2). For any `z`, asinh(z) =

asin(iz)/i.

### Example

```cpp
#include <complex>
#include <iostream>

int main()
{
    std::cout << std::fixed;
    std::complex<double> z1(0.0, -2.0);
    std::cout << "asinh" << z1 << " = " << std::asinh(z1) << '\n';

    std::complex<double> z2(-0.0, -2);
    std::cout << "asinh" << z2 << " (the other side of the cut) = "
              << std::asinh(z2) << '\n';

    // for any z, asinh(z) = asin(iz) / i
    std::complex<double> z3(1.0, 2.0);
    std::complex<double> i(0.0, 1.0);
    std::cout << "asinh" << z3 << " = " << std::asinh(z3) << '\n'
              << "asin" << z3 * i << " / i = " << std::asin(z3 * i) / i << '\n';
}
```

Output:

```text
asinh(0.000000,-2.000000) = (1.316958,-1.570796)
asinh(-0.000000,-2.000000) (the other side of the cut) = (-1.316958,-1.570796)
asinh(1.000000,2.000000) = (1.469352,1.063440)
asin(-2.000000,1.000000) / i = (1.469352,1.063440)
```

### See also

- **acosh(std::complex) (C++11)** — computes area hyperbolic cosine of a complex
  number (\({\small\operatorname{arcosh}{z}}\)arcosh(z)) (function template)
- **atanh(std::complex) (C++11)** — computes area hyperbolic tangent of a
  complex number (\({\small\operatorname{artanh}{z}}\)artanh(z)) (function
  template)
- **sinh(std::complex)** — computes hyperbolic sine of a complex number
  (\({\small\sinh{z}}\)sinh(z)) (function template)
- **asinhasinhfasinhl (C++11)(C++11)(C++11)** — computes the inverse hyperbolic
  sine (\({\small\operatorname{arsinh}{x}}\)arsinh(x)) (function)

**C documentation for `casinh`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/complex/asinh*
