# std::proj(std::complex)

```cpp
template< class T >
std::complex<T> proj( const std::complex<T>& z );  // (1) (since C++11)
Additional overloads (since C++11)
std::complex<float>       proj( float f );
std::complex<double>      proj( double f );
std::complex<long double> proj( long double f );  // (until C++23)
template< class FloatingPoint >
std::complex<FloatingPoint> proj( FloatingPoint f );  // (since C++23)
template< class Integer >
std::complex<double> proj( Integer i );  // (B)
```

1) Returns the projection of the complex number `z` onto the Riemann sphere.

For most `z`, `std::proj(z) == z`, but all complex infinities, even the numbers
   where one component is infinite and the other is NaN, become positive real
   infinity, `(INFINITY, 0.0)` or `(INFINITY, -0.0)`. The sign of the imaginary
   (zero) component is the sign of `std::imag(z)`.

A,B) Additional overloads are provided for all integer and floating-point types,
   which are treated as complex numbers with positive zero imaginary component.

### Parameters

- **z** — complex value
- **f** — floating-point value
- **i** — integer value

### Return value

1) The projection of `z` onto the Riemann sphere.

A) The projection of `std::complex(f)` onto the Riemann sphere.

B) The projection of `std::complex<double>(i)` onto the Riemann sphere.

### Notes

The `proj` function helps model the Riemann sphere by mapping all infinities to
one (give or take the sign of the imaginary zero), and should be used just
before any operation, especially comparisons, that might give spurious results
for any of the other infinities.

The additional overloads are not required to be provided exactly as (A,B). They
only need to be sufficient to ensure that for their argument `num`:

- If `num` has a standard(until C++23) floating-point type `T`, then
  `std::proj(num)` has the same effect as `std::proj(std::complex<T>(num))`.
- Otherwise, if `num` has an integer type, then `std::proj(num)` has the same
  effect as `std::proj(std::complex<double>(num))`.

### Example

```cpp
#include <complex>
#include <iostream>

int main()
{
    std::complex<double> c1(1, 2);
    std::cout << "proj" << c1 << " = " << std::proj(c1) << '\n';

    std::complex<double> c2(INFINITY, -1);
    std::cout << "proj" << c2 << " = " << std::proj(c2) << '\n';

    std::complex<double> c3(0, -INFINITY);
    std::cout << "proj" << c3 << " = " << std::proj(c3) << '\n';
}
```

Output:

```text
proj(1,2) = (1,2)
proj(inf,-1) = (inf,-0)
proj(0,-inf) = (inf,-0)
```

### See also

- **abs(std::complex)** — returns the magnitude of a complex number (function
  template)
- **norm** — returns the squared magnitude (function template)
- **polar** — constructs a complex number from magnitude and phase angle
  (function template)

**C documentation for `cproj`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/complex/proj*
