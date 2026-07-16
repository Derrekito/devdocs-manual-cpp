# std::exp, std::expf, std::expl

```cpp
float       exp ( float num );
double      exp ( double num );
long double exp ( long double num );  // (until C++23)
/* floating-point-type */
            exp ( /* floating-point-type */ num );  // (since C++23) (constexpr since C++26)
float       expf( float num );  // (2) (since C++11) (constexpr since C++26)
long double expl( long double num );  // (3) (since C++11) (constexpr since C++26)
Additional overloads (since C++11)
template< class Integer >
double      exp ( Integer num );  // (A) (constexpr since C++26)
```

1-3) Computes e (Euler's number, `2.7182818...`) raised to the given power
   `num`. The library provides overloads of `std::exp` for all cv-unqualified
   floating-point types as the type of the parameter.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
double.
*(since C++11)*

### Parameters

- **num** — floating-point or integer value

### Return value

If no errors occur, the base-e exponential of `num` (enum) is returned.

If a range error occurs due to overflow, `+HUGE_VAL`, `+HUGE_VALF`, or
`+HUGE_VALL` is returned.

If a range error occurs due to underflow, the correct result (after rounding) is
returned.

### Error handling

Errors are reported as specified in `math_errhandling`.

If the implementation supports IEEE floating-point arithmetic (IEC 60559),

- If the argument is ±0, 1 is returned.
- If the argument is -∞, +0 is returned.
- If the argument is +∞, +∞ is returned.
- If the argument is NaN, NaN is returned.

### Notes

For IEEE-compatible type double, overflow is guaranteed if 709.8 < num, and
underflow is guaranteed if num < -708.4.

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their argument `num` of integer
type, `std::exp(num)` has the same effect as
`std::exp(static_cast<double>(num))`.

### Example

```cpp
#include <cerrno>
#include <cfenv>
#include <cmath>
#include <cstring>
#include <iomanip>
#include <iostream>
#include <numbers>

// #pragma STDC FENV_ACCESS ON

consteval double approx_e()
{
    long double e{1.0};
    for (auto fac{1ull}, n{1llu}; n != 18; ++n, fac *= n)
        e += 1.0 / fac;
    return e;
}

int main()
{
    std::cout << std::setprecision(16)
              << "exp(1) = e¹ = " << std::exp(1) << '\n'
              << "numbers::e  = " << std::numbers::e << '\n'
              << "approx_e    = " << approx_e() << '\n'
              << "FV of $100, continuously compounded at 3% for 1 year = "
              << std::setprecision(6) << 100 * std::exp(0.03) << '\n';

    // special values
    std::cout << "exp(-0) = " << std::exp(-0.0) << '\n'
              << "exp(-Inf) = " << std::exp(-INFINITY) << '\n';

    // error handling
    errno = 0;
    std::feclearexcept(FE_ALL_EXCEPT);

    std::cout << "exp(710) = " << std::exp(710) << '\n';

    if (errno == ERANGE)
        std::cout << "    errno == ERANGE: " << std::strerror(errno) << '\n';
    if (std::fetestexcept(FE_OVERFLOW))
        std::cout << "    FE_OVERFLOW raised\n";
}
```

Possible output:

```text
exp(1) = e¹ = 2.718281828459045
numbers::e  = 2.718281828459045
approx_e    = 2.718281828459045
FV of $100, continuously compounded at 3% for 1 year = 103.045
exp(-0) = 1
exp(-Inf) = 0
exp(710) = inf
    errno == ERANGE: Numerical result out of range
    FE_OVERFLOW raised
```

### See also

- **exp2exp2fexp2l (C++11)(C++11)(C++11)** — returns *2* raised to the given
  power (\({\small 2^x}\)2x) (function)
- **expm1expm1fexpm1l (C++11)(C++11)(C++11)** — returns *e* raised to the given
  power, minus one (\({\small e^x-1}\)ex-1) (function)
- **loglogflogl (C++11)(C++11)** — computes natural (base *e*) logarithm
  (\({\small\ln{x}}\)ln(x)) (function)
- **exp(std::complex)** — complex base *e* exponential (function template)
- **exp(std::valarray)** — applies the function `std::exp` to each element of
  valarray (function template)

**C documentation for `exp`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/exp*
