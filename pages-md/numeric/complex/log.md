# std::log(std::complex)

```cpp
template< class T >
std::complex<T> log( const std::complex<T>& z );
```

Computes complex natural (base *e*) logarithm of a complex value `z` with a
branch cut along the negative real axis.

### Parameters

- **z** — complex value

### Return value

If no errors occur, the complex natural logarithm of `z` is returned, in the
range of a strip in the interval [−iπ, +iπ] along the imaginary axis and
mathematically unbounded along the real axis.

### Error handling and special values

Errors are reported consistent with `math_errhandling`.

If the implementation supports IEEE floating-point arithmetic,

- The function is continuous onto the branch cut taking into account the sign of
  imaginary part
- `std::log(std::conj(z)) == std::conj(std::log(z))`
- If `z` is `(-0,+0)`, the result is `(-∞,π)` and `FE_DIVBYZERO` is raised
- If `z` is `(+0,+0)`, the result is `(-∞,+0)` and `FE_DIVBYZERO` is raised
- If `z` is `(x,+∞)` (for any finite x), the result is `(+∞,π/2)`
- If `z` is `(x,NaN)` (for any finite x), the result is `(NaN,NaN)` and
  `FE_INVALID` may be raised
- If `z` is `(-∞,y)` (for any finite positive y), the result is `(+∞,π)`
- If `z` is `(+∞,y)` (for any finite positive y), the result is `(+∞,+0)`
- If `z` is `(-∞,+∞)`, the result is `(+∞,3π/4)`
- If `z` is `(+∞,+∞)`, the result is `(+∞,π/4)`
- If `z` is `(±∞,NaN)`, the result is `(+∞,NaN)`
- If `z` is `(NaN,y)` (for any finite y), the result is `(NaN,NaN)` and
  `FE_INVALID` may be raised
- If `z` is `(NaN,+∞)`, the result is `(+∞,NaN)`
- If `z` is `(NaN,NaN)`, the result is `(NaN,NaN)`

### Notes

The natural logarithm of a complex number `z` with polar coordinate components
(r,θ) equals ln r + i(θ+2nπ), with the principal value ln r + iθ.

The semantics of this function are intended to be consistent with the C function
`clog`.

### Example

```cpp
#include <cmath>
#include <complex>
#include <iostream>

int main()
{
    std::complex<double> z {0.0, 1.0}; // r = 1, θ = pi / 2
    std::cout << "2 * log" << z << " = " << 2.0 * std::log(z) << '\n';

    std::complex<double> z2 {sqrt(2.0) / 2, sqrt(2.0) / 2}; // r = 1, θ = pi / 4
    std::cout << "4 * log" << z2 << " = " << 4.0 * std::log(z2) << '\n';

    std::complex<double> z3 {-1.0, 0.0}; // r = 1, θ = pi
    std::cout << "log" << z3 << " = " << std::log(z3) << '\n';
    std::complex<double> z4 {-1.0, -0.0}; // the other side of the cut
    std::cout << "log" << z4 << " (the other side of the cut) = " << std::log(z4) << '\n';
}
```

Possible output:

```text
2 * log(0,1) = (0,3.14159)
4 * log(0.707107,0.707107) = (0,3.14159)
log(-1,0) = (0,3.14159)
log(-1,-0) (the other side of the cut) = (0,-3.14159)
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2597 | C++98 | specification mishandles signed zero imaginary parts |
      erroneous requirement removed

### See also

- **log10(std::complex)** — complex common logarithm with the branch cuts along
  the negative real axis (function template)
- **exp(std::complex)** — complex base *e* exponential (function template)
- **loglogflogl (C++11)(C++11)** — computes natural (base *e*) logarithm
  (\({\small\ln{x}}\)ln(x)) (function)
- **log(std::valarray)** — applies the function `std::log` to each element of
  valarray (function template)

**C documentation for `clog`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/complex/log*
