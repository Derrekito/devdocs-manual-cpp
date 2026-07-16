# std::abs(std::complex)

```cpp
template< class T >
T abs( const complex<T>& z );
```

Returns the magnitude of the complex number `z`.

### Parameters

- **z** — complex value

### Return value

If no errors occur, returns the absolute value (also known as norm, modulus, or
magnitude) of `z`.

Errors and special cases are handled as if the function is implemented as
`std::hypot(std::real(z), std::imag(z))`.

### Example

```cpp
#include <complex>
#include <iostream>

int main()
{
    std::complex<double> z(1, 1);
    std::cout << z << " cartesian is rho = " << std::abs(z)
              << " theta = " << std::arg(z) << " polar\n";
}
```

Output:

```text
(1,1) cartesian is rho = 1.41421 theta = 0.785398 polar
```

### See also

- **arg** — returns the phase angle (function template)
- **polar** — constructs a complex number from magnitude and phase angle
  (function template)
- **abs(int)labsllabs (C++11)** — computes absolute value of an integral value
  (\(\small{|x|}\)|x|) (function)
- **abs(float)fabsfabsffabsl (C++11)(C++11)** — absolute value of a floating
  point value (\(\small{|x|}\)|x|) (function)
- **hypothypotfhypotl (C++11)(C++11)(C++11)** — computes square root of the sum
  of the squares of two or three(since C++17) given numbers
  (\(\scriptsize{\sqrt{x^2+y^2}}\)√x2+y2),
  (\(\scriptsize{\sqrt{x^2+y^2+z^2}}\)√x2+y2+z2)(since C++17) (function)
- **abs(std::valarray)** — applies the function `abs` to each element of
  valarray (function template)

**C documentation for `cabs`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/complex/abs*
