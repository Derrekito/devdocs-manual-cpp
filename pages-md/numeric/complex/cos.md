# std::cos(std::complex)

```cpp
template< class T >
complex<T> cos( const complex<T>& z );
```

Computes complex cosine of a complex value `z`.

### Parameters

- **z** — complex value

### Return value

If no errors occur, the complex cosine of `z` is returned.

Errors and special cases are handled as if the operation is implemented by
`std::cosh(i * z)`, where `i` is the imaginary unit.

### Notes

The cosine is an entire function on the complex plane, and has no branch cuts.
Mathematical definition of the cosine is cos z =

eiz+e-iz/2.

### Example

```cpp
#include <cmath>
#include <complex>
#include <iostream>

int main()
{
    std::cout << std::fixed;
    std::complex<double> z(1.0, 0.0); // behaves like real cosine along the real line
    std::cout << "cos" << z << " = " << std::cos(z)
              << " ( cos(1) = " << std::cos(1) << ")\n";

    std::complex<double> z2(0.0, 1.0); // behaves like real cosh along the imaginary line
    std::cout << "cos" << z2 << " = " << std::cos(z2)
              << " (cosh(1) = " << std::cosh(1) << ")\n";
}
```

Output:

```text
cos(1.000000,0.000000) = (0.540302,-0.000000) ( cos(1) = 0.540302)
cos(0.000000,1.000000) = (1.543081,-0.000000) (cosh(1) = 1.543081)
```

### See also

- **sin(std::complex)** — computes sine of a complex number
  (\({\small\sin{z}}\)sin(z)) (function template)
- **tan(std::complex)** — computes tangent of a complex number
  (\({\small\tan{z}}\)tan(z)) (function template)
- **acos(std::complex) (C++11)** — computes arc cosine of a complex number
  (\({\small\arccos{z}}\)arccos(z)) (function template)
- **coscosfcosl (C++11)(C++11)** — computes cosine (\({\small\cos{x}}\)cos(x))
  (function)
- **cos(std::valarray)** — applies the function `std::cos` to each element of
  valarray (function template)

**C documentation for `ccos`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/complex/cos*
