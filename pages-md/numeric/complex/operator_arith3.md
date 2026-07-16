# operator+,-,*,/ (std::complex)

```cpp
template< class T >
std::complex<T> operator+( const std::complex<T>& lhs,
                           const std::complex<T>& rhs );  // (until C++20)
template< class T >
constexpr std::complex<T> operator+( const std::complex<T>& lhs,
                                     const std::complex<T>& rhs );  // (since C++20)
template< class T >
std::complex<T> operator+( const std::complex<T>& lhs,
                           const T& rhs );  // (until C++20)
template< class T >
constexpr std::complex<T> operator+( const std::complex<T>& lhs,
                                     const T& rhs );  // (since C++20)
template< class T >
std::complex<T> operator+( const T& lhs,
                           const std::complex<T>& rhs );  // (until C++20)
template< class T >
constexpr std::complex<T> operator+( const T& lhs,
                                     const std::complex<T>& rhs );  // (since C++20)
template< class T >
std::complex<T> operator-( const std::complex<T>& lhs,
                           const std::complex<T>& rhs );  // (until C++20)
template< class T >
constexpr std::complex<T> operator-( const std::complex<T>& lhs,
                                     const std::complex<T>& rhs );  // (since C++20)
template< class T >
std::complex<T> operator-( const std::complex<T>& lhs,
                           const T& rhs );  // (until C++20)
template< class T >
constexpr std::complex<T> operator-( const std::complex<T>& lhs,
                                     const T& rhs );  // (since C++20)
template< class T >
std::complex<T> operator-( const T& lhs,
                           const std::complex<T>& rhs );  // (until C++20)
template< class T >
constexpr std::complex<T> operator-( const T& lhs,
                                     const std::complex<T>& rhs );  // (since C++20)
template< class T >
std::complex<T> operator*( const std::complex<T>& lhs,
                           const std::complex<T>& rhs );  // (until C++20)
template< class T >
constexpr std::complex<T> operator*( const std::complex<T>& lhs,
                                     const std::complex<T>& rhs );  // (since C++20)
template< class T >
std::complex<T> operator*( const std::complex<T>& lhs,
                           const T& rhs );  // (until C++20)
template< class T >
constexpr std::complex<T> operator*( const std::complex<T>& lhs,
                                     const T& rhs );  // (since C++20)
template< class T >
std::complex<T> operator*( const T& lhs,
                           const std::complex<T>& rhs );  // (until C++20)
template< class T >
constexpr std::complex<T> operator*( const T& lhs,
                                     const std::complex<T>& rhs );  // (since C++20)
template< class T >
std::complex<T> operator/( const std::complex<T>& lhs,
                           const std::complex<T>& rhs );  // (until C++20)
template< class T >
constexpr std::complex<T> operator/( const std::complex<T>& lhs,
                                     const std::complex<T>& rhs );  // (since C++20)
template< class T >
std::complex<T> operator/( const std::complex<T>& lhs,
                           const T& rhs );  // (until C++20)
template< class T >
constexpr std::complex<T> operator/( const std::complex<T>& lhs,
                                     const T& rhs );  // (since C++20)
template< class T >
std::complex<T> operator/( const T& lhs,
                           const std::complex<T>& rhs );  // (until C++20)
template< class T >
constexpr std::complex<T> operator/( const T& lhs,
                                     const std::complex<T>& rhs );  // (since C++20)
```

Implements the binary operators for complex arithmetic and for mixed
complex/scalar arithmetic. Scalar arguments are treated as complex numbers with
the real part equal to the argument and the imaginary part set to zero.

1-3) Returns the sum of its arguments.

4-6) Returns the result of subtracting `rhs` from `lhs`.

7-9) Multiplies its arguments.

10-12) Divides `lhs` by `rhs`.

### Parameters

- **lhs, rhs** — the arguments: either both complex numbers or one complex and
  one scalar of matching type (`float`, `double`, `long double`)

### Return value

1-3) `std::complex<T>(lhs) += rhs`

4-6) `std::complex<T>(lhs) -= rhs`

7-9) `std::complex<T>(lhs) *= rhs`

10-12) `std::complex<T>(lhs) /= rhs`

### Notes

Because template argument deduction does not consider implicit conversions,
these operators cannot be used for mixed integer/complex arithmetic. In all
cases, the scalar must have the same type as the underlying type of the complex
number.

The GCC flag "-fcx-limited-range" (included by "-ffast-math") changes the
behavior of complex multiply/division by removing checks for floating point edge
cases. This impacts loop vectorization.

### Example

```cpp
#include <complex>
#include <iostream>

int main()
{
    std::complex<double> c2(2.0, 0.0);
    std::complex<double> ci(0.0, 1.0);

    std::cout << ci << " + " << c2 << " = " << ci + c2 << '\n'
              << ci << " * " << ci << " = " << ci * ci << '\n'
              << ci << " + " << c2 << " / " << ci << " = " << ci + c2 / ci << '\n'
              << 1  << " / " << ci << " = " << 1.0 / ci << '\n';

//    std::cout << 1.0f / ci; // compile error
//    std::cout << 1 / ci; // compile error
}
```

Output:

```text
(0,1) + (2,0) = (2,1)
(0,1) * (0,1) = (-1,0)
(0,1) + (2,0) / (0,1) = (0,-1)
1 / (0,1) = (0,-1)
```

### See also

- **operator+=operator-=operator*=operator/=** — compound assignment of two
  complex numbers or a complex and a scalar (public member function)
- **operator+operator-** — applies unary operators to complex numbers (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/complex/operator_arith3*
