# std::tanh, std::tanhf, std::tanhl

```cpp
float       tanh ( float num );
double      tanh ( double num );
long double tanh ( long double num );  // (until C++23)
/* floating-point-type */
            tanh ( /* floating-point-type */ num );  // (since C++23) (constexpr since C++26)
float       tanhf( float num );  // (2) (since C++11) (constexpr since C++26)
long double tanhl( long double num );  // (3) (since C++11) (constexpr since C++26)
Additional overloads (since C++11)
template< class Integer >
double      tanh ( Integer num );  // (A) (constexpr since C++26)
```

1-3) Computes the hyperbolic tangent of `num`. The library provides overloads of
   `std::tanh` for all cv-unqualified floating-point types as the type of the
   parameter.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
double.
*(since C++11)*

### Parameters

- **num** — floating-point or integer value

### Return value

If no errors occur, the hyperbolic tangent of `num` (tanh(num), or
enum-e-num/enum+e-num) is returned.

If a range error occurs due to underflow, the correct result (after rounding) is
returned.

### Error handling

Errors are reported as specified in `math_errhandling`.

If the implementation supports IEEE floating-point arithmetic (IEC 60559),

- if the argument is ±0, ±0 is returned.
- if the argument is ±∞, ±1 is returned.
- if the argument is NaN, NaN is returned.

### Notes

POSIX specifies that in case of underflow, `num` is returned unmodified, and if
that is not supported, and implementation-defined value no greater than DBL_MIN,
FLT_MIN, and LDBL_MIN is returned.

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their argument `num` of integer
type, `std::tanh(num)` has the same effect as
`std::tanh(static_cast<double>(num))`.

### Example

```cpp
#include <cmath>
#include <iostream>
#include <random>

double get_random_between(double min, double max)
{
    std::random_device rd;
    std::mt19937 gen(rd());
    return std::uniform_real_distribution<>(min, max)(gen);
}

int main()
{
    const double x = get_random_between(-1.0, 1.0);

    std::cout << std::showpos
              << "tanh(+1) = " << std::tanh(+1) << '\n'
              << "tanh(-1) = " << std::tanh(-1) << '\n'
              << "tanh(x)*sinh(2*x)-cos(2*x) = "
              << std::tanh(x) * std::sinh(2 * x) - std::cosh(2 * x) << '\n'
              // special values:
              << "tanh(+0) = " << std::tanh(+0.0) << '\n'
              << "tanh(-0) = " << std::tanh(-0.0) << '\n';
}
```

Output:

```text
tanh(+1) = +0.761594
tanh(-1) = -0.761594
tanh(x)*sinh(2*x)-cos(2*x) = -1
tanh(+0) = +0
tanh(-0) = -0
```

### See also

- **sinhsinhfsinhl (C++11)(C++11)** — computes hyperbolic sine
  (\({\small\sinh{x}}\)sinh(x)) (function)
- **coshcoshfcoshl (C++11)(C++11)** — computes hyperbolic cosine
  (\({\small\cosh{x}}\)cosh(x)) (function)
- **atanhatanhfatanhl (C++11)(C++11)(C++11)** — computes the inverse hyperbolic
  tangent (\({\small\operatorname{artanh}{x}}\)artanh(x)) (function)
- **tanh(std::complex)** — computes hyperbolic tangent of a complex number
  (\({\small\tanh{z}}\)tanh(z)) (function template)
- **tanh(std::valarray)** — applies the function `std::tanh` to each element of
  valarray (function template)

**C documentation for `tanh`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/tanh*
