# std::complex<T>::imag

```cpp
std::complex<T>
T imag() const;  // (until C++14)
constexpr T imag() const;  // (since C++14)
void imag( T value );  // (until C++20)
constexpr void imag( T value );  // (since C++20)
std::complex<float>
float imag() const;  // (until C++11)
constexpr float imag() const;  // (since C++11)
void imag( float value );  // (until C++20)
constexpr void imag( float value );  // (since C++20)
std::complex<double>
double imag() const;  // (until C++11)
constexpr double imag() const;  // (since C++11)
void imag( double value );  // (until C++20)
constexpr void imag( double value );  // (since C++20)
std::complex<long double>
long double imag() const;  // (until C++11)
constexpr long double imag() const;  // (since C++11)
void imag( long double value );  // (until C++20)
constexpr void imag( long double value );  // (since C++20)
```

Accesses the imaginary part of the complex number.

1) Returns the imaginary part.

2) Sets the imaginary part to `value`.

### Parameters

- **value** — the value to set the imaginary part to

### Return value

1) The imaginary part of `*this`.

2) (none)

### Notes

In C++11, overload (1) in `std::complex` specializations used to be specified
without const qualifier. However, in C++11, a `constexpr` specifier used in a
non-static member function implies const, and thus the behavior is as if const
is specified.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 387 | C++98 | the imaginary part could not be set directly | can be set
      directly via a new `imag` overload

### See also

- **imag** — returns the imaginary part (function template)
- **real** — accesses the real part of the complex number (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/numeric/complex/imag*
