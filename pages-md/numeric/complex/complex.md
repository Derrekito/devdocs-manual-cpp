# std::complex<T>::complex

```cpp
std::complex<T>
complex( const T& re = T(), const T& im = T() );  // (until C++14)
constexpr complex( const T& re = T(), const T& im = T() );  // (since C++14)
complex( const complex& other );  // (until C++14)
constexpr complex( const complex& other );  // (since C++14) (until C++23)
constexpr complex( const complex& other ) = default;  // (since C++23)
template< class X >
complex( const complex<X>& other );  // (until C++14)
template< class X >
constexpr complex( const complex<X>& other );  // (since C++14) (until C++23)
template< class X >
constexpr explicit(/* see below */) complex( const complex<X>& other );  // (since C++23)
std::complex<float>
complex( float re = 0.0f, float im = 0.0f );  // (until C++11)
constexpr complex( float re = 0.0f, float im = 0.0f );  // (since C++11)
constexpr complex( const complex<float>& other ) = default;  // (2) (since C++20)
explicit complex( const complex<double>& other );
explicit complex( const complex<long double>& other );  // (until C++11)
constexpr explicit complex( const complex<double>& other );
constexpr explicit complex( const complex<long double>& other );  // (since C++11)
std::complex<double>
complex( double re = 0.0, double im = 0.0 );  // (until C++11)
constexpr complex( double re = 0.0, double im = 0.0 );  // (since C++11)
constexpr complex( const complex<double>& other ) = default;  // (2) (since C++20)
complex( const complex<float>& other );
explicit complex( const complex<long double>& other );  // (until C++11)
constexpr complex( const complex<float>& other );
constexpr explicit complex( const complex<long double>& other );  // (since C++11)
std::complex<long double>
complex( long double re = 0.0L, long double im = 0.0L );  // (until C++11)
constexpr complex( long double re = 0.0L, long double im = 0.0L );  // (since C++11)
constexpr complex( const complex<long double>& other ) = default;  // (2) (since C++20)
complex( const complex<float>& other );
complex( const complex<double>& other );  // (until C++11)
constexpr complex( const complex<float>& other );
constexpr complex( const complex<double>& other );  // (since C++11)
```

Constructs the std::complex object. The standard explicit specializations
(`std::complex<float>`, `std::complex<double>` and `std::complex<long double>`)
have different constructor declarations from the main template.(until C++23)

1) Constructs the complex number from real part `re` and imaginary part `im`.

2) Copy constructor. Constructs the object with the copy of the contents of
   `other`. The copy constructors are implicitly declared in the standard
   explicit specializations.(until C++20)

3) Converting constructor. Constructs the object from a complex number of a
   different type. The main template provides a converting constructor template,
   while each standard explicit specialization provides two non-template
   constructors for the two other standard explicit specializations. The
   non-template constructors are converting constructors (i.e. non-explicit) if
   and only if the conversions of the real and imaginary parts are not
   narrowing. (until C++23) For the main template, the expression inside
   explicit evaluates to false if and only if the floating-point conversion rank
   of `T` is greater than or equal to the floating-point conversion rank of `X`.
   (since C++23)

### Parameters

- **re** — the real part
- **im** — the imaginary part
- **other** — another complex number to use as source

### Notes

Since C++23, the copy constructor is required to be trivial in order to satisfy
the TriviallyCopyable requirement, but implementations generally make it trivial
in all modes.

### See also

- **operator=** — assigns the contents (public member function)
- **operator""ifoperator""ioperator""il (C++14)** — a `std::complex` literal
  representing purely imaginary number (function)

**C documentation for `CMPLX`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/complex/complex*
