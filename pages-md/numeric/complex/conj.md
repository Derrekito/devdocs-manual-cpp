# std::conj(std::complex)

```cpp
template< class T >
std::complex<T> conj( const std::complex<T>& z );  // (until C++20)
template< class T >
constexpr std::complex<T> conj( const std::complex<T>& z );  // (since C++20)
Additional overloads (since C++11)
std::complex<float>       conj( float f );
std::complex<double>      conj( double f );
std::complex<long double> conj( long double f );  // (until C++20)
constexpr std::complex<float>       conj( float f );
constexpr std::complex<double>      conj( double f );
constexpr std::complex<long double> conj( long double f );  // (since C++20) (until C++23)
template< class FloatingPoint >
constexpr std::complex<FloatingPoint> conj( FloatingPoint f );  // (since C++23)
template< class Integer >
constexpr std::complex<double> conj( Integer i );  // (until C++20)
template< class Integer >
constexpr std::complex<double> conj( Integer i );  // (since C++20)
```

1) Computes the complex conjugate of `z` by reversing the sign of the imaginary
   part.

A,B) Additional overloads are provided for all integer and floating-point types,
which are treated as complex numbers with zero imaginary component.
*(since C++11)*

### Parameters

- **z** — complex value
- **f** — floating-point value
- **i** — integer value

### Return value

1) The complex conjugate of `z`.

A) `std::complex(f)`.

B) `std::complex<double>(i)`.

### Notes

The additional overloads are not required to be provided exactly as (A,B). They
only need to be sufficient to ensure that for their argument `num`:

- If `num` has a standard(until C++23) floating-point type `T`, then
  `std::conj(num)` has the same effect as `std::conj(std::complex<T>(num))`.
- Otherwise, if `num` has an integer type, then `std::conj(num)` has the same
  effect as `std::conj(std::complex<double>(num))`.

### Example

```cpp
#include <complex>
#include <iostream>

int main()
{
    std::complex<double> z(1.0, 2.0);
    std::cout << "The conjugate of " << z << " is " << std::conj(z) << '\n'
              << "Their product is " << z * std::conj(z) << '\n';
}
```

Output:

```text
The conjugate of (1,2) is (1,-2)
Their product is (5,0)
```

### See also

- **abs(std::complex)** — returns the magnitude of a complex number (function
  template)
- **norm** — returns the squared magnitude (function template)
- **polar** — constructs a complex number from magnitude and phase angle
  (function template)

**C documentation for `conj`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/complex/conj*
