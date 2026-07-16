# std::complex<T>::operator=

```cpp
All specializations
complex& operator=( const complex& cx );  // (until C++20)
constexpr complex& operator=( const complex& cx );  // (since C++20)
template< class X >
complex& operator=( const std::complex<X>& cx );  // (until C++20)
template< class X >
constexpr complex& operator=( const std::complex<X>& cx );  // (since C++20)
Primary template complex<T>
complex& operator=( const T& x );  // (until C++20)
constexpr complex& operator=( const T& x );  // (since C++20)
Specializations for standard floating-point types complex<F> (until C++23)
complex& operator=( F x );  // (until C++20)
constexpr complex& operator=( F x );  // (since C++20) (until C++23)
```

Assigns new values to the contents.

1,2) Assigns `cx.real()` and `cx.imag()` to the real and the imaginary parts of
   the complex number respectively. The copy assignment operator (1) is trivial
   when `T` is a floating-point type.(since C++23)

3) Assigns `x` to the real part of the complex number. Imaginary part is set to
   zero.

### Parameters

- **x** — value to assign
- **cx** — complex value to assign

### Return value

`*this`

### Notes

The copy assignment operator is required to be trivial since C++23, but
implementations generally make it trivial in all modes.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3935 | C++98 | the description of overload (2) was missing | added

### See also

- **(constructor)** — constructs a complex number (public member function)
- **operator""ifoperator""ioperator""il (C++14)** — a `std::complex` literal
  representing purely imaginary number (function)

---
*Source: https://en.cppreference.com/w/cpp/numeric/complex/operator%3D*
