# std::atanh(std::complex)

```cpp
template< class T >
complex<T> atanh( const complex<T>& z );  // (since C++11)
```

Computes the complex arc hyperbolic tangent of `z` with branch cuts outside the
interval [−1; +1] along the real axis.

### Parameters

- **z** — complex value

### Return value

If no errors occur, the complex arc hyperbolic tangent of `z` is returned, in
the range of a half-strip mathematically unbounded along the real axis and in
the interval [−iπ/2; +iπ/2] along the imaginary axis.

### Error handling and special values

Errors are reported consistent with `math_errhandling`.

If the implementation supports IEEE floating-point arithmetic,

- `std::atanh(std::conj(z)) == std::conj(std::atanh(z))`
- `std::atanh(-z) == -std::atanh(z)`
- If `z` is `(+0,+0)`, the result is `(+0,+0)`
- If `z` is `(+0,NaN)`, the result is `(+0,NaN)`
- If `z` is `(+1,+0)`, the result is `(+∞,+0)` and `FE_DIVBYZERO` is raised
- If `z` is `(x,+∞)` (for any finite positive x), the result is `(+0,π/2)`
- If `z` is `(x,NaN)` (for any finite nonzero x), the result is `(NaN,NaN)` and
  `FE_INVALID` may be raised
- If `z` is `(+∞,y)` (for any finite positive y), the result is `(+0,π/2)`
- If `z` is `(+∞,+∞)`, the result is `(+0,π/2)`
- If `z` is `(+∞,NaN)`, the result is `(+0,NaN)`
- If `z` is `(NaN,y)` (for any finite y), the result is `(NaN,NaN)` and
  `FE_INVALID` may be raised
- If `z` is `(NaN,+∞)`, the result is `(±0,π/2)` (the sign of the real part is
  unspecified)
- If `z` is `(NaN,NaN)`, the result is `(NaN,NaN)`

### Notes

Although the C++ standard names this function "complex arc hyperbolic tangent",
the inverse functions of the hyperbolic functions are the area functions. Their
argument is the area of a hyperbolic sector, not an arc. The correct name is
"complex inverse hyperbolic tangent", and, less common, "complex area hyperbolic
tangent".

Inverse hyperbolic tangent is a multivalued function and requires a branch cut
on the complex plane. The branch cut is conventionally placed at the line
segments (-∞,-1] and [+1,+∞) of the real axis. The mathematical definition of
the principal value of the inverse hyperbolic tangent is atanh z =

ln(1+z) - ln(1-z)/2.
For any `z`, atanh(z) = atan(iz)/i.

### Example

```cpp
#include <complex>
#include <iostream>

int main()
{
    std::cout << std::fixed;
    std::complex<double> z1(2.0, 0.0);
    std::cout << "atanh" << z1 << " = " << std::atanh(z1) << '\n';

    std::complex<double> z2(2.0, -0.0);
    std::cout << "atanh" << z2 << " (the other side of the cut) = "
              << std::atanh(z2) << '\n';

    // for any z, atanh(z) = atanh(iz) / i
    std::complex<double> z3(1.0, 2.0);
    std::complex<double> i(0.0, 1.0);
    std::cout << "atanh" << z3 << " = " << std::atanh(z3) << '\n'
              << "atan" << z3 * i << " / i = " << std::atan(z3 * i) / i << '\n';
}
```

Output:

```text
atanh(2.000000,0.000000) = (0.549306,1.570796)
atanh(2.000000,-0.000000) (the other side of the cut) = (0.549306,-1.570796)
atanh(1.000000,2.000000) = (0.173287,1.178097)
atan(-2.000000,1.000000) / i = (0.173287,1.178097)
```

### See also

- **asinh(std::complex) (C++11)** — computes area hyperbolic sine of a complex
  number (\({\small\operatorname{arsinh}{z}}\)arsinh(z)) (function template)
- **acosh(std::complex) (C++11)** — computes area hyperbolic cosine of a complex
  number (\({\small\operatorname{arcosh}{z}}\)arcosh(z)) (function template)
- **tanh(std::complex)** — computes hyperbolic tangent of a complex number
  (\({\small\tanh{z}}\)tanh(z)) (function template)
- **atanhatanhfatanhl (C++11)(C++11)(C++11)** — computes the inverse hyperbolic
  tangent (\({\small\operatorname{artanh}{x}}\)artanh(x)) (function)

**C documentation for `catanh`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/complex/atanh*
