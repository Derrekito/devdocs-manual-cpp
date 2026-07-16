# std::norm(std::complex)

```cpp
template< class T >
T norm( const std::complex<T>& z );  // (until C++20)
template< class T >
constexpr T norm( const std::complex<T>& z );  // (since C++20)
Additional overloads (since C++11)
float       norm( float f );
double      norm( double f );
long double norm( long double f );  // (until C++20)
constexpr float       norm( float f );
constexpr double      norm( double f );
constexpr long double norm( long double f );  // (since C++20) (until C++23)
template< class FloatingPoint >
constexpr FloatingPoint norm( FloatingPoint f );  // (since C++23)
template< class Integer >
double norm( Integer i );  // (until C++20)
template< class Integer >
constexpr double norm( Integer i );  // (since C++20)
```

1) Returns the squared magnitude of the complex number `z`.

A,B) Additional overloads are provided for all integer and floating-point types,
which are treated as complex numbers with zero imaginary component.
*(since C++11)*

### Parameters

- **z** — complex value
- **f** — floating-point value
- **i** — integer value

### Return value

1) The squared magnitude of `z`.

A) The square of `f`.

B) The square of `i`.

### Notes

The norm calculated by this function is also known as field norm or absolute
square.

The Euclidean norm of a complex number is provided by `std::abs`, which is more
costly to compute. In some situations, it may be replaced by `std::norm`, for
example, if `abs(z1) > abs(z2)` then `norm(z1) > norm(z2)`.

The additional overloads are not required to be provided exactly as (A,B). They
only need to be sufficient to ensure that for their argument `num`:

- If `num` has a standard(until C++23) floating-point type `T`, then
  `std::norm(num)` has the same effect as `std::norm(std::complex<T>(num))`.
- Otherwise, if `num` has an integer type, then `std::norm(num)` has the same
  effect as `std::norm(std::complex<double>(num))`.

### Example

```cpp
#include <cassert>
#include <complex>
#include <iostream>

int main()
{
    constexpr std::complex<double> z {3.0, 4.0};
    static_assert(std::norm(z) == (z.real() * z.real() + z.imag() * z.imag()));
    static_assert(std::norm(z) == (z * std::conj(z)));
           assert(std::norm(z) == (std::abs(z) * std::abs(z)));
    std::cout << "std::norm(" << z << ") = " << std::norm(z) << '\n';
}
```

Output:

```text
std::norm((3,4)) = 25
```

### See also

- **abs(std::complex)** — returns the magnitude of a complex number (function
  template)
- **conj** — returns the complex conjugate (function template)
- **polar** — constructs a complex number from magnitude and phase angle
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/complex/norm*
