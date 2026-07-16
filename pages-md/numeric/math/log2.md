# std::log2, std::log2f, std::log2l

```cpp
float       log2 ( float num );
double      log2 ( double num );
long double log2 ( long double num );  // (until C++23)
/* floating-point-type */
            log2 ( /* floating-point-type */ num );  // (since C++23) (constexpr since C++26)
float       log2f( float num );  // (2) (since C++11) (constexpr since C++26)
long double log2l( long double num );  // (3) (since C++11) (constexpr since C++26)
Additional overloads (since C++11)
template< class Integer >
double      log2 ( Integer num );  // (A) (constexpr since C++26)
```

1-3) Computes the binary (base-*2*) logarithm of `num`. The library provides
   overloads of `std::log2` for all cv-unqualified floating-point types as the
   type of the parameter.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
double.
*(since C++11)*

### Parameters

- **num** — floating-point or integer value

### Return value

If no errors occur, the base-*2* logarithm of `num` (log2(num) or lb(num)) is
returned.

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

For integer `num`, the binary logarithm can be interpreted as the zero-based
index of the most significant 1 bit in the input.

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their argument `num` of integer
type, `std::log2(num)` has the same effect as
`std::log2(static_cast<double>(num))`.

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
    std::cout << "log2(65536) = " << std::log2(65536) << '\n'
              << "log2(0.125) = " << std::log2(0.125) << '\n'
              << "log2(0x020f) = " << std::log2(0x020f)
              << " (highest set bit is in position 9)\n"
              << "base-5 logarithm of 125 = "
              << std::log2(125) / std::log2(5) << '\n';

    // special values
    std::cout << "log2(1) = " << std::log2(1) << '\n'
              << "log2(+Inf) = " << std::log2(INFINITY) << '\n';

    // error handling
    errno = 0;
    std::feclearexcept(FE_ALL_EXCEPT);

    std::cout << "log2(0) = " << std::log2(0) << '\n';

    if (errno == ERANGE)
        std::cout << "    errno == ERANGE: " << std::strerror(errno) << '\n';
    if (std::fetestexcept(FE_DIVBYZERO))
        std::cout << "    FE_DIVBYZERO raised\n";
}
```

Possible output:

```text
log2(65536) = 16
log2(0.125) = -3
log2(0x020f) = 9.04166 (highest set bit is in position 9)
base-5 logarithm of 125 = 3
log2(1) = 0
log2(+Inf) = inf
log2(0) = -inf
    errno == ERANGE: Numerical result out of range
    FE_DIVBYZERO raised
```

### See also

- **loglogflogl (C++11)(C++11)** — computes natural (base *e*) logarithm
  (\({\small\ln{x}}\)ln(x)) (function)
- **log10log10flog10l (C++11)(C++11)** — computes common (base *10*) logarithm
  (\({\small\log_{10}{x}}\)log10(x)) (function)
- **log1plog1pflog1pl (C++11)(C++11)(C++11)** — natural logarithm (to base *e*)
  of 1 plus the given number (\({\small\ln{(1+x)}}\)ln(1+x)) (function)
- **exp2exp2fexp2l (C++11)(C++11)(C++11)** — returns *2* raised to the given
  power (\({\small 2^x}\)2x) (function)

**C documentation for `log2`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/log2*
