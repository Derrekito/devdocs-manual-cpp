# std::log10, std::log10f, std::log10l

```cpp
float       log10 ( float num );
double      log10 ( double num );
long double log10 ( long double num );  // (until C++23)
/* floating-point-type */
            log10 ( /* floating-point-type */ num );  // (since C++23) (constexpr since C++26)
float       log10f( float num );  // (2) (since C++11) (constexpr since C++26)
long double log10l( long double num );  // (3) (since C++11) (constexpr since C++26)
Additional overloads (since C++11)
template< class Integer >
double      log10 ( Integer num );  // (A) (constexpr since C++26)
```

1-3) Computes the common (base-*10*) logarithm of `num`. The library provides
   overloads of `std::log10` for all cv-unqualified floating-point types as the
   type of the parameter.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
double.
*(since C++11)*

### Parameters

- **num** — floating-point or integer value

### Return value

If no errors occur, the common (base-*10*) logarithm of `num` (log10(num) or
lg(num)) is returned.

If a domain error occurs, an implementation-defined value is returned (NaN where
supported).

If a pole error occurs, `-HUGE_VAL`, `-HUGE_VALF`, or `-HUGE_VALL` is returned.

### Error handling

Errors are reported as specified in `math_errhandling`.

Domain error occurs if `num` is less than zero.

Pole error may occur if `num` is zero.

If the implementation supports IEEE floating-point arithmetic (IEC 60559),

- If the argument is ±0, -∞ is returned and `FE_DIVBYZERO` is raised.
- If the argument is 1, +0 is returned.
- If the argument is negative, NaN is returned and `FE_INVALID` is raised.
- If the argument is +∞, +∞ is returned.
- If the argument is NaN, NaN is returned.

### Notes

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their argument `num` of integer
type, `std::log10(num)` has the same effect as
`std::log10(static_cast<double>(num))`.

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
    std::cout << "log10(1000) = " << std::log10(1000) << '\n'
              << "log10(0.001) = " << std::log10(0.001) << '\n'
              << "base-5 logarithm of 125 = "
              << std::log10(125) / std::log10(5) << '\n';

    // special values
    std::cout << "log10(1) = " << std::log10(1) << '\n'
              << "log10(+Inf) = " << std::log10(INFINITY) << '\n';

    // error handling
    errno = 0;
    std::feclearexcept(FE_ALL_EXCEPT);

    std::cout << "log10(0) = " << std::log10(0) << '\n';

    if (errno == ERANGE)
        std::cout << "    errno == ERANGE: " << std::strerror(errno) << '\n';
    if (std::fetestexcept(FE_DIVBYZERO))
        std::cout << "    FE_DIVBYZERO raised\n";
}
```

Possible output:

```text
log10(1000) = 3
log10(0.001) = -3
base-5 logarithm of 125 = 3
log10(1) = 0
log10(+Inf) = inf
log10(0) = -inf
    errno == ERANGE: Numerical result out of range
    FE_DIVBYZERO raised
```

### See also

- **loglogflogl (C++11)(C++11)** — computes natural (base *e*) logarithm
  (\({\small\ln{x}}\)ln(x)) (function)
- **log2log2flog2l (C++11)(C++11)(C++11)** — base 2 logarithm of the given
  number (\({\small\log_{2}{x}}\)log2(x)) (function)
- **log1plog1pflog1pl (C++11)(C++11)(C++11)** — natural logarithm (to base *e*)
  of 1 plus the given number (\({\small\ln{(1+x)}}\)ln(1+x)) (function)
- **log10(std::complex)** — complex common logarithm with the branch cuts along
  the negative real axis (function template)
- **log10(std::valarray)** — applies the function `std::log10` to each element
  of valarray (function template)

**C documentation for `log10`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/log10*
