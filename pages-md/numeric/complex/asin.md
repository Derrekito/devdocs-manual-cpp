# std::asin(std::complex)

```cpp
template< class T >
std::complex<T> asin( const std::complex<T>& z );  // (since C++11)
```

Computes complex arc sine of a complex value `z`. Branch cut exists outside the
interval [−1, +1] along the real axis.

### Parameters

- **z** — complex value

### Return value

If no errors occur, complex arc sine of `z` is returned, in the range of a strip
unbounded along the imaginary axis and in the interval [−π/2, +π/2] along the
real axis.

Errors and special cases are handled as if the operation is implemented by `-i *
std::asinh(i * z)`, where `i` is the imaginary unit.

### Notes

Inverse sine (or arc sine) is a multivalued function and requires a branch cut
on the complex plane. The branch cut is conventionally placed at the line
segments (-∞,-1) and (1,∞) of the real axis.

The mathematical definition of the principal value of arc sine is \(\small
\arcsin z = -{\rm i}\ln({\rm i}z+\sqrt{1-z^2})\)arcsin z = -*i*ln(*i*z + √1-z2).
For any `z`, \(\small{ \arcsin(z) = \arccos(-z) - \frac{\pi}{2} }\)asin(z) =
acos(-z) -

π/2.

### Example

```cpp
#include <cmath>
#include <complex>
#include <iostream>

int main()
{
    std::cout << std::fixed;
    std::complex<double> z1(-2.0, 0.0);
    std::cout << "asin" << z1 << " = " << std::asin(z1) << '\n';

    std::complex<double> z2(-2.0, -0.0);
    std::cout << "asin" << z2 << " (the other side of the cut) = "
              << std::asin(z2) << '\n';

    // for any z, asin(z) = acos(−z) − pi / 2
    const double pi = std::acos(-1);
    std::complex<double> z3 = std::acos(z2) - pi / 2;
    std::cout << "sin(acos" << z2 << " - pi / 2) = " << std::sin(z3) << '\n';
}
```

Output:

```text
asin(-2.000000,0.000000) = (-1.570796,1.316958)
asin(-2.000000,-0.000000) (the other side of the cut) = (-1.570796,-1.316958)
sin(acos(-2.000000,-0.000000) - pi / 2) = (2.000000,0.000000)
```

### See also

- **acos(std::complex) (C++11)** — computes arc cosine of a complex number
  (\({\small\arccos{z}}\)arccos(z)) (function template)
- **atan(std::complex) (C++11)** — computes arc tangent of a complex number
  (\({\small\arctan{z}}\)arctan(z)) (function template)
- **sin(std::complex)** — computes sine of a complex number
  (\({\small\sin{z}}\)sin(z)) (function template)
- **asinasinfasinl (C++11)(C++11)** — computes arc sine
  (\({\small\arcsin{x}}\)arcsin(x)) (function)
- **asin(std::valarray)** — applies the function `std::asin` to each element of
  valarray (function template)

**C documentation for `casin`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/complex/asin*
