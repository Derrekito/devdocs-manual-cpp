# std::arg(std::complex)

```cpp
template< class T >
T           arg( const std::complex<T>& z );  // (1)
Additional overloads (since C++11)
float       arg( float f );
double      arg( double f );
long double arg( long double f );  // (until C++23)
template< class FloatingPoint >
FloatingPoint
            arg( FloatingPoint f );  // (since C++23)
template< class Integer >
double      arg( Integer i );  // (B)
```

1) Calculates the phase angle (in radians) of the complex number `z`.

A,B) Additional overloads are provided for all integer and floating-point types,
which are treated as complex numbers with zero imaginary component.
*(since C++11)*

### Parameters

- **z** — complex value
- **f** — floating-point value
- **i** — integer value

### Return value

1) `std::atan2(std::imag(z), std::real(z))`. If no errors occur, this is the
   phase angle of `z` in the interval [−π; π].

A) Zero if `f` is positive or +0, π if `f` is negative or -0, NaN otherwise.

B) Zero if `i` is non-negative, π if it is negative.

### Notes

The additional overloads are not required to be provided exactly as (A,B). They
only need to be sufficient to ensure that for their argument `num`:

- If `num` has a standard(until C++23) floating-point type `T`, then
  `std::arg(num)` has the same effect as `std::arg(std::complex<T>(num))`.
- Otherwise, if `num` has an integer type, then `std::arg(num)` has the same
  effect as `std::arg(std::complex<double>(num))`.

### Example

```cpp
#include <complex>
#include <iostream>

int main()
{
    std::complex<double> z1(1, 0);
    std::complex<double> z2(0, 0);
    std::complex<double> z3(0, 1);
    std::complex<double> z4(-1, 0);
    std::complex<double> z5(-1, -0.0);
    double f = 1.;
    int i = -1;

    std::cout << "phase angle of " << z1 << " is " << std::arg(z1) << '\n'
              << "phase angle of " << z2 << " is " << std::arg(z2) << '\n'
              << "phase angle of " << z3 << " is " << std::arg(z3) << '\n'
              << "phase angle of " << z4 << " is " << std::arg(z4) << '\n'
              << "phase angle of " << z5 << " is " << std::arg(z5) << " "
                 "(the other side of the cut)\n"
              << "phase angle of " << f << " is " << std::arg(f) << '\n'
              << "phase angle of " << i << " is " << std::arg(i) << '\n';

}
```

Output:

```text
phase angle of (1,0) is 0
phase angle of (0,0) is 0
phase angle of (0,1) is 1.5708
phase angle of (-1,0) is 3.14159
phase angle of (-1,-0) is -3.14159 (the other side of the cut)
phase angle of 1 is 0
phase angle of -1 is 3.14159
```

### See also

- **abs(std::complex)** — returns the magnitude of a complex number (function
  template)
- **polar** — constructs a complex number from magnitude and phase angle
  (function template)
- **atan2atan2fatan2l (C++11)(C++11)** — arc tangent, using signs to determine
  quadrants (function)
- **atan2(std::valarray)** — applies the function `std::atan2` to a valarray and
  a value (function template)

**C documentation for `carg`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/complex/arg*
