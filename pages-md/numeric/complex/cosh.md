# std::cosh(std::complex)

```cpp
template< class T >
complex<T> cosh( const complex<T>& z );  // (since C++11)
```

Computes complex hyperbolic cosine of a complex value `z`.

### Parameters

- **z** — complex value

### Return value

If no errors occur, complex hyperbolic cosine of `z` is returned.

### Error handling and special values

Errors are reported consistent with `math_errhandling`.

If the implementation supports IEEE floating-point arithmetic,

- `std::cosh(std::conj(z)) == std::conj(std::cosh(z))`
- `std::cosh(z) == std::cosh(-z)`
- If `z` is `(+0,+0)`, the result is `(1,+0)`
- If `z` is `(+0,+∞)`, the result is `(NaN,±0)` (the sign of the imaginary part
  is unspecified) and `FE_INVALID` is raised
- If `z` is `(+0,NaN)`, the result is `(NaN,±0)` (the sign of the imaginary part
  is unspecified)
- If `z` is `(x,+∞)` (for any finite non-zero x), the result is `(NaN,NaN)` and
  `FE_INVALID` is raised
- If `z` is `(x,NaN)` (for any finite non-zero x), the result is `(NaN,NaN)` and
  `FE_INVALID` may be raised
- If `z` is `(+∞,+0)`, the result is `(+∞,+0)`
- If `z` is `(+∞,y)` (for any finite non-zero y), the result is `+∞cis(y)`
- If `z` is `(+∞,+∞)`, the result is `(±∞,NaN)` (the sign of the real part is
  unspecified) and `FE_INVALID` is raised
- If `z` is `(+∞,NaN)`, the result is `(+∞,NaN)`
- If `z` is `(NaN,+0)`, the result is `(NaN,±0)` (the sign of the imaginary part
  is unspecified)
- If `z` is `(NaN,+y)` (for any finite non-zero y), the result is `(NaN,NaN)`
  and `FE_INVALID` may be raised
- If `z` is `(NaN,NaN)`, the result is `(NaN,NaN)`

where cis(y) is cos(y) + i sin(y).

### Notes

Mathematical definition of hyperbolic cosine is cosh z = ez+e-z/2.

Hyperbolic cosine is an entire function in the complex plane and has no branch
cuts. It is periodic with respect to the imaginary component, with period 2πi.

### Examples

```cpp
#include <cmath>
#include <complex>
#include <iostream>

int main()
{
    std::cout << std::fixed;
    std::complex<double> z(1.0, 0.0); // behaves like real cosh along the real line
    std::cout << "cosh" << z << " = " << std::cosh(z)
              << " (cosh(1) = " << std::cosh(1) << ")\n";

    std::complex<double> z2(0.0, 1.0); // behaves like real cosine along the imaginary line
    std::cout << "cosh" << z2 << " = " << std::cosh(z2)
              << " ( cos(1) = " << std::cos(1) << ")\n";
}
```

Output:

```text
cosh(1.000000,0.000000) = (1.543081,0.000000) (cosh(1) = 1.543081)
cosh(0.000000,1.000000) = (0.540302,0.000000) ( cos(1) = 0.540302)
```

### See also

- **sinh(std::complex)** — computes hyperbolic sine of a complex number
  (\({\small\sinh{z}}\)sinh(z)) (function template)
- **tanh(std::complex)** — computes hyperbolic tangent of a complex number
  (\({\small\tanh{z}}\)tanh(z)) (function template)
- **acosh(std::complex) (C++11)** — computes area hyperbolic cosine of a complex
  number (\({\small\operatorname{arcosh}{z}}\)arcosh(z)) (function template)
- **coshcoshfcoshl (C++11)(C++11)** — computes hyperbolic cosine
  (\({\small\cosh{x}}\)cosh(x)) (function)
- **cosh(std::valarray)** — applies the function `std::cosh` to each element of
  valarray (function template)

**C documentation for `ccosh`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/complex/cosh*
