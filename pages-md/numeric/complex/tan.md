# std::tan(std::complex)

```cpp
template< class T >
complex<T> tan( const complex<T>& z );
```

Computes complex tangent of a complex value `z`.

### Parameters

- **z** — complex value

### Return value

If no errors occur, the complex tangent of `z` is returned.

Errors and special cases are handled as if the operation is implemented by `-i *
std::tanh(i * z)`, where `i` is the imaginary unit.

### Notes

Tangent is an analytical function on the complex plain and has no branch cuts.
It is periodic with respect to the real component, with period πi, and has poles
of the first order along the real line, at coordinates (π(1/2 + n), 0). However
no common floating-point representation is able to represent π/2 exactly, thus
there is no value of the argument for which a pole error occurs. Mathematical
definition of the tangent is tan z =

i(e-iz-eiz)/e-iz+eiz.

### Example

```cpp
#include <cmath>
#include <complex>
#include <iostream>

int main()
{
    std::cout << std::fixed;
    std::complex<double> z(1.0, 0.0); // behaves like real tangent along the real line
    std::cout << "tan" << z << " = " << std::tan(z)
              << " ( tan(1) = " << std::tan(1) << ")\n";

    std::complex<double> z2(0.0, 1.0); // behaves like tanh along the imaginary line
    std::cout << "tan" << z2 << " = " << std::tan(z2)
              << " (tanh(1) = " << std::tanh(1) << ")\n";
}
```

Output:

```text
tan(1.000000,0.000000) = (1.557408,0.000000) ( tan(1) = 1.557408)
tan(0.000000,1.000000) = (0.000000,0.761594) (tanh(1) = 0.761594)
```

### See also

- **sin(std::complex)** — computes sine of a complex number
  (\({\small\sin{z}}\)sin(z)) (function template)
- **cos(std::complex)** — computes cosine of a complex number
  (\({\small\cos{z}}\)cos(z)) (function template)
- **atan(std::complex) (C++11)** — computes arc tangent of a complex number
  (\({\small\arctan{z}}\)arctan(z)) (function template)
- **tantanftanl (C++11)(C++11)** — computes tangent (\({\small\tan{x}}\)tan(x))
  (function)
- **tan(std::valarray)** — applies the function `std::tan` to each element of
  valarray (function template)

**C documentation for `ctan`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/complex/tan*
