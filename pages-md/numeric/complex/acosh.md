# std::acosh(std::complex)

```cpp
template< class T >
complex<T> acosh( const complex<T>& z );  // (since C++11)
```

Computes complex arc hyperbolic cosine of a complex value `z` with branch cut at
values less than 1 along the real axis.

### Parameters

- **z** — complex value

### Return value

If no errors occur, the complex arc hyperbolic cosine of `z` is returned, in the
range of a half-strip of nonnegative values along the real axis and in the
interval [−iπ; +iπ] along the imaginary axis.

### Error handling and special values

Errors are reported consistent with `math_errhandling`.

If the implementation supports IEEE floating-point arithmetic,

- `std::acosh(std::conj(z)) == std::conj(std::acosh(z))`.
- If `z` is `(±0,+0)`, the result is `(+0,π/2)`.
- If `z` is `(x,+∞)` (for any finite x), the result is `(+∞,π/2)`.
- If `z` is `(x,NaN)` (for any[1] finite x), the result is `(NaN,NaN)` and
  `FE_INVALID` may be raised.
- If `z` is `(-∞,y)` (for any positive finite y), the result is `(+∞,π)`.
- If `z` is `(+∞,y)` (for any positive finite y), the result is `(+∞,+0)`.
- If `z` is `(-∞,+∞)`, the result is `(+∞,3π/4)`.
- If `z` is `(±∞,NaN)`, the result is `(+∞,NaN)`.
- If `z` is `(NaN,y)` (for any finite y), the result is `(NaN,NaN)` and
  `FE_INVALID` may be raised.
- If `z` is `(NaN,+∞)`, the result is `(+∞,NaN)`.
- If `z` is `(NaN,NaN)`, the result is `(NaN,NaN)`.

1. per C11 DR471, this holds for non-zero x only. If `z` is `(0,NaN)`, the
   result should be `(NaN,π/2)`.

### Notes

Although the C++ standard names this function "complex arc hyperbolic cosine",
the inverse functions of the hyperbolic functions are the area functions. Their
argument is the area of a hyperbolic sector, not an arc. The correct name is
"complex inverse hyperbolic cosine", and, less common, "complex area hyperbolic
cosine".

Inverse hyperbolic cosine is a multivalued function and requires a branch cut on
the complex plane. The branch cut is conventionally placed at the line segment
(-∞,+1) of the real axis.

The mathematical definition of the principal value of the inverse hyperbolic
cosine is acosh z = ln(z + √z+1 √z-1). For any `z`, acosh(z) =

√z-1/√1-z acos(z), or simply i acos(z) in the upper half of the complex plane.

### Example

```cpp
#include <complex>
#include <iostream>

int main()
{
    std::cout << std::fixed;
    std::complex<double> z1(0.5, 0);
    std::cout << "acosh" << z1 << " = " << std::acosh(z1) << '\n';

    std::complex<double> z2(0.5, -0.0);
    std::cout << "acosh" << z2 << " (the other side of the cut) = "
              << std::acosh(z2) << '\n';

    // in upper half-plane, acosh = i acos
    std::complex<double> z3(1, 1), i(0, 1);
    std::cout << "acosh" << z3 << " = " << std::acosh(z3) << '\n'
              << "i*acos" << z3 << " = " << i*std::acos(z3) << '\n';
}
```

Output:

```text
acosh(0.500000,0.000000) = (0.000000,-1.047198)
acosh(0.500000,-0.000000) (the other side of the cut) = (0.000000,1.047198)
acosh(1.000000,1.000000) = (1.061275,0.904557)
i*acos(1.000000,1.000000) = (1.061275,0.904557)
```

### See also

- **acos(std::complex) (C++11)** — computes arc cosine of a complex number
  (\({\small\arccos{z}}\)arccos(z)) (function template)
- **asinh(std::complex) (C++11)** — computes area hyperbolic sine of a complex
  number (\({\small\operatorname{arsinh}{z}}\)arsinh(z)) (function template)
- **atanh(std::complex) (C++11)** — computes area hyperbolic tangent of a
  complex number (\({\small\operatorname{artanh}{z}}\)artanh(z)) (function
  template)
- **cosh(std::complex)** — computes hyperbolic cosine of a complex number
  (\({\small\cosh{z}}\)cosh(z)) (function template)
- **acoshacoshfacoshl (C++11)(C++11)(C++11)** — computes the inverse hyperbolic
  cosine (\({\small\operatorname{arcosh}{x}}\)arcosh(x)) (function)

**C documentation for `cacosh`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/complex/acosh*
