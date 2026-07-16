# std::complex<T>::operator+(unary), operator-(unary)

```cpp
template< class T >
std::complex<T> operator+( const std::complex<T>& val );  // (until C++20)
template< class T >
constexpr std::complex<T> operator+( const std::complex<T>& val );  // (since C++20)
template< class T >
std::complex<T> operator-( const std::complex<T>& val );  // (until C++20)
template< class T >
constexpr std::complex<T> operator-( const std::complex<T>& val );  // (since C++20)
```

Implements the analogs of the unary arithmetic operators for complex numbers.

1) Returns the value of its argument

2) Negates the argument

### Parameters

- **val** — the complex number argument

### Return value

1) a copy of the argument, `std::complex<T>(val)`

2) negated argument, `std::complex<T>(-val.real(), -val.imag())`

### See also

- **operator+operator-operator*operator/** — performs complex number arithmetic
  on two complex values or a complex and a scalar (function template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/complex/operator_arith2*
