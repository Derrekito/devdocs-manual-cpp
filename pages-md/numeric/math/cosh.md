# std::cosh, std::coshf, std::coshl

```cpp
float       cosh ( float num );
double      cosh ( double num );
long double cosh ( long double num );  // (until C++23)
/* floating-point-type */
            cosh ( /* floating-point-type */ num );  // (since C++23) (constexpr since C++26)
float       coshf( float num );  // (2) (since C++11) (constexpr since C++26)
long double coshl( long double num );  // (3) (since C++11) (constexpr since C++26)
Additional overloads (since C++11)
template< class Integer >
double      cosh ( Integer num );  // (A) (constexpr since C++26)
```

1-3) Computes the hyperbolic cosine of `num`. The library provides overloads of
   `std::cosh` for all cv-unqualified floating-point types as the type of the
   parameter.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
double.
*(since C++11)*

### Parameters

- **num** — floating-point or integer value

### Return value

If no errors occur, the hyperbolic cosine of `num` (cosh(num), or enum+e-num/2)
is returned.

If a range error due to overflow occurs, `+HUGE_VAL`, `+HUGE_VALF`, or
`+HUGE_VALL` is returned.

### Error handling

Errors are reported as specified in `math_errhandling`.

If the implementation supports IEEE floating-point arithmetic (IEC 60559),

- if the argument is ±0, 1 is returned.
- If the argument is ±∞, +∞ is returned.
- if the argument is NaN, NaN is returned.

### Notes

For the IEEE-compatible type double, if |num| > 710.5, then `std::cosh(num)`
overflows.

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their argument `num` of integer
type, `std::cosh(num)` has the same effect as
`std::cosh(static_cast<double>(num))`.

### Example

```cpp
#include <cerrno>
#include <cfenv>
#include <cmath>
#include <cstring>
#include <iostream>
// #pragma STDC FENV_ACCESS ON

int main()
{
    const double x = 42;

    std::cout << "cosh(1) = " << std::cosh(1) << '\n'
              << "cosh(-1) = " << std::cosh(-1) << '\n'
              << "log(sinh(" << x << ")+cosh(" << x << ")) = "
              << std::log(std::sinh(x) + std::cosh(x)) << '\n';

    // special values
    std::cout << "cosh(+0) = " << std::cosh(0.0) << '\n'
              << "cosh(-0) = " << std::cosh(-0.0) << '\n';

    // error handling
    errno=0;
    std::feclearexcept(FE_ALL_EXCEPT);

    std::cout << "cosh(710.5) = " << std::cosh(710.5) << '\n';

    if (errno == ERANGE)
        std::cout << "    errno == ERANGE: " << std::strerror(errno) << '\n';
    if (std::fetestexcept(FE_OVERFLOW))
        std::cout << "    FE_OVERFLOW raised\n";
}
```

Possible output:

```text
cosh(1) = 1.54308
cosh(-1) = 1.54308
log(sinh(42)+cosh(42)) = 42
cosh(+0) = 1
cosh(-0) = 1
cosh(710.5) = inf
    errno == ERANGE: Numerical result out of range
    FE_OVERFLOW raised
```

### See also

- **sinhsinhfsinhl (C++11)(C++11)** — computes hyperbolic sine
  (\({\small\sinh{x}}\)sinh(x)) (function)
- **tanhtanhftanhl (C++11)(C++11)** — computes hyperbolic tangent
  (\({\small\tanh{x}}\)tanh(x)) (function)
- **acoshacoshfacoshl (C++11)(C++11)(C++11)** — computes the inverse hyperbolic
  cosine (\({\small\operatorname{arcosh}{x}}\)arcosh(x)) (function)
- **cosh(std::complex)** — computes hyperbolic cosine of a complex number
  (\({\small\cosh{z}}\)cosh(z)) (function template)
- **cosh(std::valarray)** — applies the function `std::cosh` to each element of
  valarray (function template)

**C documentation for `cosh`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/cosh*
