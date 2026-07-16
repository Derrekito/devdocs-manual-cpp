# std::complex

```cpp
template< class T >
class complex;  // (1)
template<> class complex<float>;  // (2) (until C++23)
template<> class complex<double>;  // (3) (until C++23)
template<> class complex<long double>;  // (4) (until C++23)
```

Specializations of `std::complex` for cv-unqualified standard(until C++23)
floating-point types are TriviallyCopyable(since C++23) LiteralTypes for
representing and manipulating complex number.

### Template parameters

- **T** — the type of the real and imaginary parts. The behavior is unspecified
  (and may fail to compile) if `T` is not a cv-unqualified standard(until C++23)
  floating-point type and undefined if `T` is not NumericType.

### Member types

- **`value_type`** — `T`

### Member functions

- **(constructor)** — constructs a complex number (public member function)
- **operator=** — assigns the contents (public member function)
- **real** — accesses the real part of the complex number (public member
  function)
- **imag** — accesses the imaginary part of the complex number (public member
  function)
- **operator+=operator-=operator*=operator/=** — compound assignment of two
  complex numbers or a complex and a scalar (public member function)

### Non-member functions

- **operator+operator-** — applies unary operators to complex numbers (function
  template)
- **operator+operator-operator*operator/** — performs complex number arithmetic
  on two complex values or a complex and a scalar (function template)
- **operator==operator!= (removed in C++20)** — compares two complex numbers or
  a complex and a scalar (function template)
- **operator<<operator>>** — serializes and deserializes a complex number
  (function template)
- **get(std::complex) (C++26)** — obtains a reference to real or imaginary part
  from a `std::complex` (function template)
- **real** — returns the real part (function template)
- **imag** — returns the imaginary part (function template)
- **abs(std::complex)** — returns the magnitude of a complex number (function
  template)
- **arg** — returns the phase angle (function template)
- **norm** — returns the squared magnitude (function template)
- **conj** — returns the complex conjugate (function template)
- **proj (C++11)** — returns the projection onto the Riemann sphere (function
  template)
- **polar** — constructs a complex number from magnitude and phase angle
  (function template)

**Exponential functions**

- **exp(std::complex)** — complex base *e* exponential (function template)
- **log(std::complex)** — complex natural logarithm with the branch cuts along
  the negative real axis (function template)
- **log10(std::complex)** — complex common logarithm with the branch cuts along
  the negative real axis (function template)

**Power functions**

- **pow(std::complex)** — complex power, one or both arguments may be a complex
  number (function template)
- **sqrt(std::complex)** — complex square root in the range of the right
  half-plane (function template)

**Trigonometric functions**

- **sin(std::complex)** — computes sine of a complex number
  (\({\small\sin{z}}\)sin(z)) (function template)
- **cos(std::complex)** — computes cosine of a complex number
  (\({\small\cos{z}}\)cos(z)) (function template)
- **tan(std::complex)** — computes tangent of a complex number
  (\({\small\tan{z}}\)tan(z)) (function template)
- **asin(std::complex) (C++11)** — computes arc sine of a complex number
  (\({\small\arcsin{z}}\)arcsin(z)) (function template)
- **acos(std::complex) (C++11)** — computes arc cosine of a complex number
  (\({\small\arccos{z}}\)arccos(z)) (function template)
- **atan(std::complex) (C++11)** — computes arc tangent of a complex number
  (\({\small\arctan{z}}\)arctan(z)) (function template)

**Hyperbolic functions**

- **sinh(std::complex)** — computes hyperbolic sine of a complex number
  (\({\small\sinh{z}}\)sinh(z)) (function template)
- **cosh(std::complex)** — computes hyperbolic cosine of a complex number
  (\({\small\cosh{z}}\)cosh(z)) (function template)
- **tanh(std::complex)** — computes hyperbolic tangent of a complex number
  (\({\small\tanh{z}}\)tanh(z)) (function template)
- **asinh(std::complex) (C++11)** — computes area hyperbolic sine of a complex
  number (\({\small\operatorname{arsinh}{z}}\)arsinh(z)) (function template)
- **acosh(std::complex) (C++11)** — computes area hyperbolic cosine of a complex
  number (\({\small\operatorname{arcosh}{z}}\)arcosh(z)) (function template)
- **atanh(std::complex) (C++11)** — computes area hyperbolic tangent of a
  complex number (\({\small\operatorname{artanh}{z}}\)artanh(z)) (function
  template)

### Helper types

- **std::tuple_size<std::complex> (C++26)** — obtains the number of components
  of a `std::complex` (class template specialization)
- **std::tuple_element<std::complex> (C++26)** — obtains the underlying real and
  imaginary number type of a `std::complex` (class template specialization)

### Array-oriented access

For any object `z` of type `std::complex<T>`, `reinterpret_cast<T(&)[2]>(z)[0]`
is the real part of `z` and `reinterpret_cast<T(&)[2]>(z)[1]` is the imaginary
part of `z`.

For any pointer to an element of an array of `std::complex<T>` named `p` and any
valid array index `i`, `reinterpret_cast<T*>(p)[2 * i]` is the real part of the
complex number `p[i]`, and `reinterpret_cast<T*>(p)[2 * i + 1]` is the imaginary
part of the complex number `p[i]`.

The intent of this requirement is to preserve binary compatibility between the
C++ library complex number types and the C language complex number types (and
arrays thereof), which have an identical object representation requirement.

### Implementation notes

In order to satisfy the requirements of array-oriented access, an implementation
is constrained to store the real and imaginary parts of a `std::complex`
specialization in separate and adjacent memory locations. Possible declarations
for its non-static data members include:

- an array of type `value_type[2]`, with the first element holding the real part
  and the second element holding the imaginary part (e.g. Microsoft Visual
  Studio);
- a single member of type `value_type _Complex` (encapsulating the corresponding
  C language complex number type) (e.g. GNU libstdc++);
- two members of type `value_type`, with the same member access, holding the
  real and the imaginary parts respectively (e.g. LLVM libc++).

An implementation cannot declare additional non-static data members that would
occupy storage disjoint from the real and imaginary parts, and must ensure that
the class template specialization does not contain any padding bit. The
implementation must also ensure that optimizations to array access account for
the possibility that a pointer to `value_type` may be aliasing a `std::complex`
specialization or array thereof.

### Literals

- **operator""ifoperator""ioperator""il (C++14)** — a `std::complex` literal
  representing purely imaginary number (function)

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_constexpr_complex` | 201711L | (C++20) | Constexpr simple complex
      mathematical functions in `<complex>`
  202306L | (C++26) | More constexpr for `<complex>`

### Example

```cpp
#include <cmath>
#include <complex>
#include <iomanip>
#include <iostream>

int main()
{
    using namespace std::complex_literals;
    std::cout << std::fixed << std::setprecision(1);

    std::complex<double> z1 = 1i * 1i; // imaginary unit squared
    std::cout << "i * i = " << z1 << '\n';

    std::complex<double> z2 = std::pow(1i, 2); // imaginary unit squared
    std::cout << "pow(i, 2) = " << z2 << '\n';

    const double PI = std::acos(-1); // or std::numbers::pi in C++20
    std::complex<double> z3 = std::exp(1i * PI); // Euler's formula
    std::cout << "exp(i * pi) = " << z3 << '\n';

    std::complex<double> z4 = 1.0 + 2i, z5 = 1.0 - 2i; // conjugates
    std::cout << "(1 + 2i) * (1 - 2i) = " << z4 * z5 << '\n';
}
```

Output:

```text
i * i = (-1.0,0.0)
pow(i, 2) = (-1.0,0.0)
exp(i * pi) = (-1.0,0.0)
(1 + 2i) * (1 - 2i) = (5.0,0.0)
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 387 | C++98 | `std::complex` was not guaranteed to be compatible with C
      `complex` | guaranteed to be compatible

### See also

**C documentation for Complex number arithmetic**

---
*Source: https://en.cppreference.com/w/cpp/numeric/complex*
