# Standard library header <complex>

This header is part of the numeric library.

### Classes

- **complex** — a complex number type (class template)

### Functions

**Operations**

- **operator+operator-** — applies unary operators to complex numbers (function
  template)
- **operator+operator-operator*operator/** — performs complex number arithmetic
  on two complex values or a complex and a scalar (function template)
- **operator==operator!= (removed in C++20)** — compares two complex numbers or
  a complex and a scalar (function template)
- **operator<<operator>>** — serializes and deserializes a complex number
  (function template)
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

**Literals**

- **operator""ifoperator""ioperator""il (C++14)** — a `std::complex` literal
  representing purely imaginary number (function)

### Synopsis

```cpp
namespace std {
    template<class T> class complex;

    template<> class complex<float>;
    template<> class complex<double>;
    template<> class complex<long double>;

    // operators:
    template<class T> constexpr complex<T> operator+(
        const complex<T>&, const complex<T>&);
    template<class T> constexpr complex<T> operator+(const complex<T>&, const T&);
    template<class T> constexpr complex<T> operator+(const T&, const complex<T>&);

    template<class T> constexpr complex<T> operator-(
        const complex<T>&, const complex<T>&);
    template<class T> constexpr complex<T> operator-(const complex<T>&, const T&);
    template<class T> constexpr complex<T> operator-(const T&, const complex<T>&);

    template<class T> constexpr complex<T> operator*(
        const complex<T>&, const complex<T>&);
    template<class T> constexpr complex<T> operator*(const complex<T>&, const T&);
    template<class T> constexpr complex<T> operator*(const T&, const complex<T>&);

    template<class T> constexpr complex<T> operator/(
        const complex<T>&, const complex<T>&);
    template<class T> constexpr complex<T> operator/(const complex<T>&, const T&);
    template<class T> constexpr complex<T> operator/(const T&, const complex<T>&);

    template<class T> constexpr complex<T> operator+(const complex<T>&);
    template<class T> constexpr complex<T> operator-(const complex<T>&);

    template<class T> constexpr bool operator==(const complex<T>&, const complex<T>&);
    template<class T> constexpr bool operator==(const complex<T>&, const T&);
    template<class T> constexpr bool operator==(const T&, const complex<T>&);

    template<class T> constexpr bool operator!=(const complex<T>&, const complex<T>&);
    template<class T> constexpr bool operator!=(const complex<T>&, const T&);
    template<class T> constexpr bool operator!=(const T&, const complex<T>&);

    template<class T, class CharT, class Traits>
    basic_istream<CharT, Traits>&
    operator>>(basic_istream<CharT, Traits>&, complex<T>&);

    template<class T, class CharT, class Traits>
    basic_ostream<CharT, Traits>&
    operator<<(basic_ostream<CharT, Traits>&, const complex<T>&);

    // values:
    template<class T> constexpr T real(const complex<T>&);
    template<class T> constexpr T imag(const complex<T>&);

    template<class T> T abs(const complex<T>&);
    template<class T> T arg(const complex<T>&);
    template<class T> constexpr T norm(const complex<T>&);

    template<class T> constexpr complex<T> conj(const complex<T>&);
    template<class T> complex<T> proj(const complex<T>&);
    template<class T> complex<T> polar(const T&, const T& = 0);

    // transcendentals:
    template<class T> complex<T> acos(const complex<T>&);
    template<class T> complex<T> asin(const complex<T>&);
    template<class T> complex<T> atan(const complex<T>&);

    template<class T> complex<T> acosh(const complex<T>&);
    template<class T> complex<T> asinh(const complex<T>&);
    template<class T> complex<T> atanh(const complex<T>&);

    template<class T> complex<T> cos  (const complex<T>&);
    template<class T> complex<T> cosh (const complex<T>&);
    template<class T> complex<T> exp  (const complex<T>&);
    template<class T> complex<T> log  (const complex<T>&);
    template<class T> complex<T> log10(const complex<T>&);

    template<class T> complex<T> pow(const complex<T>&, const T&);
    template<class T> complex<T> pow(const complex<T>&, const complex<T>&);
    template<class T> complex<T> pow(const T&, const complex<T>&);

    template<class T> complex<T> sin (const complex<T>&);
    template<class T> complex<T> sinh(const complex<T>&);
    template<class T> complex<T> sqrt(const complex<T>&);
    template<class T> complex<T> tan (const complex<T>&);
    template<class T> complex<T> tanh(const complex<T>&);

    // complex literals:
    inline namespace literals {
        inline namespace complex_literals {
            constexpr complex<long double> operator""il(long double);
            constexpr complex<long double> operator""il(unsigned long long);
            constexpr complex<double> operator""i(long double);
            constexpr complex<double> operator""i(unsigned long long);
            constexpr complex<float> operator""if(long double);
            constexpr complex<float> operator""if(unsigned long long);
        }
    }
}
```

#### Class `std::complex`

```cpp
template<class T>
class complex {
public:
    typedef T value_type;
    constexpr complex(const T& re = T(), const T& im = T());
    constexpr complex(const complex&) = default;
    template<class X> constexpr explicit(/* see constructor page */)
        complex(const complex<X>&);

    constexpr T real() const;
    constexpr void real(T);
    constexpr T imag() const;
    constexpr void imag(T);

    constexpr complex<T>& operator= (const T&);
    constexpr complex<T>& operator+=(const T&);
    constexpr complex<T>& operator-=(const T&);
    constexpr complex<T>& operator*=(const T&);
    constexpr complex<T>& operator/=(const T&);

    constexpr complex& operator=(const complex&);
    template<class X> constexpr complex<T>& operator= (const complex<X>&);
    template<class X> constexpr complex<T>& operator+=(const complex<X>&);
    template<class X> constexpr complex<T>& operator-=(const complex<X>&);
    template<class X> constexpr complex<T>& operator*=(const complex<X>&);
    template<class X> constexpr complex<T>& operator/=(const complex<X>&);
};
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 79 | C++98 | the default argument of the second parameter of `polar` was
      missing in the synopsis | added

---
*Source: https://en.cppreference.com/w/cpp/header/complex*
