# std::acos(std::complex)

```cpp
template< class T >
complex<T> acos( const complex<T>& z );  // (since C++11)
```

Computes complex arc cosine of a complex value `z`. Branch cuts exist outside
the interval [−1, +1] along the real axis.

### Parameters

- **z** — complex value

### Return value

If no errors occur, complex arc cosine of `z` is returned, in the range of a
strip unbounded along the imaginary axis and in the interval [0, +π] along the
real axis.

### Error handling and special values

Errors are reported consistent with `math_errhandling`.

If the implementation supports IEEE floating-point arithmetic,

- `std::acos(std::conj(z)) == std::conj(std::acos(z))`
- If `z` is `(±0,+0)`, the result is `(π/2,-0)`
- If `z` is `(±0,NaN)`, the result is `(π/2,NaN)`
- If `z` is `(x,+∞)` (for any finite x), the result is `(π/2,-∞)`
- If `z` is `(x,NaN)` (for any nonzero finite x), the result is `(NaN,NaN)` and
  `FE_INVALID` may be raised.
- If `z` is `(-∞,y)` (for any positive finite y), the result is `(π,-∞)`
- If `z` is `(+∞,y)` (for any positive finite y), the result is `(+0,-∞)`
- If `z` is `(-∞,+∞)`, the result is `(3π/4,-∞)`
- If `z` is `(+∞,+∞)`, the result is `(π/4,-∞)`
- If `z` is `(±∞,NaN)`, the result is `(NaN,±∞)` (the sign of the imaginary part
  is unspecified)
- If `z` is `(NaN,y)` (for any finite y), the result is `(NaN,NaN)` and
  `FE_INVALID` may be raised
- If `z` is `(NaN,+∞)`, the result is `(NaN,-∞)`
- If `z` is `(NaN,NaN)`, the result is `(NaN,NaN)`

### Notes

Inverse cosine (or arc cosine) is a multivalued function and requires a branch
cut on the complex plane. The branch cut is conventionally placed at the line
segments (-∞,-1) and (1,∞) of the real axis. The mathematical definition of the
principal value of arc cosine is acos z =

1/2π + *i*ln(*i*z + √1-z2).

For any `z`, acos(z) = π - acos(-z).

### Example

```cpp
#include <cmath>
#include <complex>
#include <iostream>

int main()
{
    std::cout << std::fixed;
    std::complex<double> z1(-2.0, 0.0);
    std::cout << "acos" << z1 << " = " << std::acos(z1) << '\n';

    std::complex<double> z2(-2.0, -0.0);
    std::cout << "acos" << z2 << " (the other side of the cut) = "
              << std::acos(z2) << '\n';

    // for any z, acos(z) = pi - acos(-z)
    const double pi = std::acos(-1);
    std::complex<double> z3 = pi - std::acos(z2);
    std::cout << "cos(pi - acos" << z2 << ") = " << std::cos(z3) << '\n';
}
```

Output:

```text
acos(-2.000000,0.000000) = (3.141593,-1.316958)
acos(-2.000000,-0.000000) (the other side of the cut) = (3.141593,1.316958)
cos(pi - acos(-2.000000,-0.000000)) = (2.000000,0.000000)
```

### See also

- **asin(std::complex) (C++11)** — computes arc sine of a complex number
  (\({\small\arcsin{z}}\)arcsin(z)) (function template)
- **atan(std::complex) (C++11)** — computes arc tangent of a complex number
  (\({\small\arctan{z}}\)arctan(z)) (function template)
- **cos(std::complex)** — computes cosine of a complex number
  (\({\small\cos{z}}\)cos(z)) (function template)
- **acosacosfacosl (C++11)(C++11)** — computes arc cosine
  (\({\small\arccos{x}}\)arccos(x)) (function)
- **acos(std::valarray)** — applies the function `std::acos` to each element of
  valarray (function template)

**C documentation for `cacos`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/complex/acos*
