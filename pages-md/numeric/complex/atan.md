# std::atan(std::complex)

```cpp
template< class T >
complex<T> atan( const complex<T>& z );  // (since C++11)
```

Computes complex arc tangent of a complex value `z`. Branch cut exists outside
the interval [−i, +i] along the imaginary axis.

### Parameters

- **z** — complex value

### Return value

If no errors occur, complex arc tangent of `z` is returned, in the range of a
strip unbounded along the imaginary axis and in the interval [−π/2, +π/2] along
the real axis.

Errors and special cases are handled as if the operation is implemented by `-i *
std::atanh(i * z)`, where `i` is the imaginary unit.

### Notes

Inverse tangent (or arc tangent) is a multivalued function and requires a branch
cut on the complex plane. The branch cut is conventionally placed at the line
segments (-∞i,-i) and (+i,+∞i) of the imaginary axis. The mathematical
definition of the principal value of inverse tangent is atan z = -

1/2 i [ln(1 - iz) - ln (1 + iz)].

### Example

```cpp
#include <cmath>
#include <complex>
#include <iostream>

int main()
{
    std::cout << std::fixed;
    std::complex<double> z1(0.0, 2.0);
    std::cout << "atan" << z1 << " = " << std::atan(z1) << '\n';

    std::complex<double> z2(-0.0, 2.0);
    std::cout << "atan" << z2 << " (the other side of the cut) = "
              << std::atan(z2) << '\n';

    std::complex<double> z3(0.0, INFINITY);
    std::cout << "2 * atan" << z3 << " = " << 2.0 * std::atan(z3) << '\n';
}
```

Output:

```text
atan(0.000000,2.000000) = (1.570796,0.549306)
atan(-0.000000,2.000000) (the other side of the cut) = (-1.570796,0.549306)
2 * atan(0.000000,inf) = (3.141593,0.000000)
```

### See also

- **asin(std::complex) (C++11)** — computes arc sine of a complex number
  (\({\small\arcsin{z}}\)arcsin(z)) (function template)
- **acos(std::complex) (C++11)** — computes arc cosine of a complex number
  (\({\small\arccos{z}}\)arccos(z)) (function template)
- **tan(std::complex)** — computes tangent of a complex number
  (\({\small\tan{z}}\)tan(z)) (function template)
- **atanatanfatanl (C++11)(C++11)** — computes arc tangent
  (\({\small\arctan{x}}\)arctan(x)) (function)
- **atan(std::valarray)** — applies the function `std::atan` to each element of
  valarray (function template)

**C documentation for `catan`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/complex/atan*
