# std::lgamma, std::lgammaf, std::lgammal

```cpp
float       lgamma ( float num );
double      lgamma ( double num );
long double lgamma ( long double num );  // (until C++23)
/* floating-point-type */
            lgamma ( /* floating-point-type */ num );  // (since C++23) (constexpr since C++26)
float       lgammaf( float num );  // (2) (since C++11) (constexpr since C++26)
long double lgammal( long double num );  // (3) (since C++11) (constexpr since C++26)
Additional overloads (since C++11)
template< class Integer >
double      lgamma ( Integer num );  // (A) (constexpr since C++26)
```

1-3) Computes the natural logarithm of the absolute value of the gamma function
   of `num`. The library provides overloads of `std::lgamma` for all
   cv-unqualified floating-point types as the type of the parameter.(since
   C++23)

A) Additional overloads are provided for all integer types, which are treated as
double.
*(since C++11)*

### Parameters

- **num** — floating-point or integer value

### Return value

If no errors occur, the value of the logarithm of the gamma function of `num`,
that is \(\log_{e}|{\int_0^\infty t^{num-1} e^{-t}
\mathsf{d}t}|\)loge|∫∞0*t*num-1 *e*-t d*t*|, is returned.

If a pole error occurs, `+HUGE_VAL`, `+HUGE_VALF`, or `+HUGE_VALL` is returned.

If a range error due to overflow occurs, `±HUGE_VAL`, `±HUGE_VALF`, or
`±HUGE_VALL` is returned.

### Error handling

Errors are reported as specified in `math_errhandling`.

If `num` is zero or is an integer less than zero, a pole error may occur.

If the implementation supports IEEE floating-point arithmetic (IEC 60559),

- If the argument is 1, +0 is returned.
- If the argument is 2, +0 is returned.
- If the argument is ±0, +∞ is returned and `FE_DIVBYZERO` is raised.
- If the argument is a negative integer, +∞ is returned and `FE_DIVBYZERO` is
  raised.
- If the argument is ±∞, +∞ is returned.
- If the argument is NaN, NaN is returned.

### Notes

If `num` is a natural number, `std::lgamma(num)` is the logarithm of the
factorial of `num - 1`.

The POSIX version of `lgamma` is not thread-safe: each execution of the function
stores the sign of the gamma function of `num` in the static external variable
`signgam`. Some implementations provide `lgamma_r`, which takes a pointer to
user-provided storage for `singgam` as the second parameter, and is thread-safe.

There is a non-standard function named `gamma` in various implementations, but
its definition is inconsistent. For example, glibc and 4.2BSD version of `gamma`
executes `lgamma`, but 4.4BSD version of `gamma` executes `tgamma`.

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their argument `num` of integer
type, `std::lgamma(num)` has the same effect as
`std::lgamma(static_cast<double>(num))`.

### Example

```cpp
#include <cerrno>
#include <cfenv>
#include <cmath>
#include <cstring>
#include <iostream>

// #pragma STDC FENV_ACCESS ON

const double pi = std::acos(-1); // or std::numbers::pi since C++20

int main()
{
    std::cout << "lgamma(10) = " << std::lgamma(10)
              << ", log(9!) = " << std::log(std::tgamma(10))
              << ", exp(lgamma(10)) = " << std::exp(std::lgamma(10)) << '\n'
              << "lgamma(0.5) = " << std::lgamma(0.5)
              << ", log(sqrt(pi)) = " << std::log(std::sqrt(pi)) << '\n';

    // special values
    std::cout << "lgamma(1) = " << std::lgamma(1) << '\n'
              << "lgamma(+Inf) = " << std::lgamma(INFINITY) << '\n';

    // error handling
    errno = 0;
    std::feclearexcept(FE_ALL_EXCEPT);

    std::cout << "lgamma(0) = " << std::lgamma(0) << '\n';

    if (errno == ERANGE)
        std::cout << "    errno == ERANGE: " << std::strerror(errno) << '\n';
    if (std::fetestexcept(FE_DIVBYZERO))
        std::cout << "    FE_DIVBYZERO raised\n";
}
```

Output:

```text
lgamma(10) = 12.8018, log(9!) = 12.8018, exp(lgamma(10)) = 362880
lgamma(0.5) = 0.572365, log(sqrt(pi)) = 0.572365
lgamma(1) = 0
lgamma(+Inf) = inf
lgamma(0) = inf
    errno == ERANGE: Numerical result out of range
    FE_DIVBYZERO raised
```

### See also

- **tgammatgammaftgammal (C++11)(C++11)(C++11)** — gamma function (function)

**C documentation for `lgamma`**

### External links

  Weisstein, Eric W. "Log Gamma Function." From MathWorld — A Wolfram Web
      Resource.

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/lgamma*
