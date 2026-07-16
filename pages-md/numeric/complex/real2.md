# std::real(std::complex)

```cpp
template< class T >
T real( const std::complex<T>& z );  // (until C++14)
template< class T >
constexpr T real( const std::complex<T>& z );  // (since C++14)
Additional overloads (since C++11)
float       real( float f );
double      real( double f );
long double real( long double f );  // (until C++14)
constexpr float       real( float f );
constexpr double      real( double f );
constexpr long double real( long double f );  // (since C++14) (until C++23)
template< class FloatingPoint >
constexpr FloatingPoint real( FloatingPoint f );  // (since C++23)
template< class Integer >
double real( Integer i );  // (until C++14)
template< class Integer >
constexpr double real( Integer i );  // (since C++14)
```

1) Returns the real part of the complex number `z`, i.e. `z.real()`.

A,B) Additional overloads are provided for all integer and floating-point types,
which are treated as complex numbers with zero imaginary part.
*(since C++11)*

### Parameters

- **z** — complex value
- **f** — floating-point value
- **i** — integer value

### Return value

1) The real part of `z`.

A) `f`.

B) `static_cast<double>(i)`.

### Notes

The additional overloads are not required to be provided exactly as (A,B). They
only need to be sufficient to ensure that for their argument `num`:

- If `num` has a standard(until C++23) floating-point type `T`, then
  `std::real(num)` has the same effect as `std::real(std::complex<T>(num))`.
- Otherwise, if `num` has an integer type, then `std::real(num)` has the same
  effect as `std::real(std::complex<double>(num))`.

### See also

- **real** — accesses the real part of the complex number (public member
  function)
- **imag** — returns the imaginary part (function template)

**C documentation for `creal`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/complex/real2*
