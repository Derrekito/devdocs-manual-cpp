# std::complex<T>::real

```cpp
std::complex<T>
T real() const;  // (until C++14)
constexpr T real() const;  // (since C++14)
void real( T value );  // (until C++20)
constexpr void real( T value );  // (since C++20)
std::complex<float>
float real() const;  // (until C++11)
constexpr float real() const;  // (since C++11)
void real( float value );  // (until C++20)
constexpr void real( float value );  // (since C++20)
std::complex<double>
double real() const;  // (until C++11)
constexpr double real() const;  // (since C++11)
void real( double value );  // (until C++20)
constexpr void real( double value );  // (since C++20)
std::complex<long double>
long double real() const;  // (until C++11)
constexpr long double real() const;  // (since C++11)
void real( long double value );  // (until C++20)
constexpr void real( long double value );  // (since C++20)
```

Accesses the real part of the complex number.

1) Returns the real part.

2) Sets the real part to `value`.

### Parameters

- **value** — the value to set the real part to

### Return value

1) The real part of `*this`.

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
  LWG 387 | C++98 | the real part could not be set directly | can be set
      directly via a new `real` overload

### See also

- **real** — returns the real part (function template)
- **imag** — accesses the imaginary part of the complex number (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/numeric/complex/real*
