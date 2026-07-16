# std::modf, std::modff, std::modfl

```cpp
float       modf ( float num, float* iptr );
double      modf ( double num, double* iptr );
long double modf ( long double num, long double* iptr );  // (until C++23)
constexpr /* floating-point-type */
            modf ( /* floating-point-type */ num,
                   /* floating-point-type */* iptr );  // (since C++23)
float       modff( float num, float* iptr );  // (2) (since C++11) (constexpr since C++23)
long double modfl( long double num, long double* iptr );  // (3) (since C++11) (constexpr since C++23)
Additional overloads (since C++11)
template< class Integer >
double      modf ( Integer num, double* iptr );  // (A) (constexpr since C++23)
```

1-3) Decomposes given floating point value `num` into integral and fractional
   parts, each having the same type and sign as `num`. The integral part (in
   floating-point format) is stored in the object pointed to by `iptr`. The
   library provides overloads of `std::modf` for all cv-unqualified
   floating-point types as the type of the parameter `num` and the pointed-to
   type of `iptr`.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
double.
*(since C++11)*

### Parameters

- **num** — floating-point or integer value
- **iptr** — pointer to floating-point value to store the integral part to

### Return value

If no errors occur, returns the fractional part of `num` with the same sign as
`num`. The integral part is put into the value pointed to by `iptr`.

The sum of the returned value and the value stored in `*iptr` gives `num`
(allowing for rounding).

### Error handling

This function is not subject to any errors specified in `math_errhandling`.

If the implementation supports IEEE floating-point arithmetic (IEC 60559),

- If `num` is ±0, ±0 is returned, and ±0 is stored in `*iptr`.
- If `num` is ±∞, ±0 is returned, and ±∞ is stored in `*iptr`.
- If `num` is NaN, NaN is returned, and NaN is stored in `*iptr`.
- The returned value is exact, the current rounding mode is ignored.

### Notes

This function behaves as if implemented as follows:

```cpp
double modf(double num, double* iptr)
{
#pragma STDC FENV_ACCESS ON
    int save_round = std::fegetround();
    std::fesetround(FE_TOWARDZERO);
    *iptr = std::nearbyint(num);
    std::fesetround(save_round);
    return std::copysign(std::isinf(num) ? 0.0 : num - (*iptr), num);
}
```

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their argument `num` of integer
type, `std::modf(num, iptr)` has the same effect as
`std::modf(static_cast<double>(num), iptr)`.

### Example

Compares different floating-point decomposition functions:

```cpp
#include <cmath>
#include <iostream>
#include <limits>

int main()
{
    double f = 123.45;
    std::cout << "Given the number " << f << " or " << std::hexfloat
              << f << std::defaultfloat << " in hex,\n";

    double f3;
    double f2 = std::modf(f, &f3);
    std::cout << "modf() makes " << f3 << " + " << f2 << '\n';

    int i;
    f2 = std::frexp(f, &i);
    std::cout << "frexp() makes " << f2 << " * 2^" << i << '\n';

    i = std::ilogb(f);
    std::cout << "logb()/ilogb() make " << f / std::scalbn(1.0, i) << " * "
              << std::numeric_limits<double>::radix
              << "^" << std::ilogb(f) << '\n';

    // special values
    f2 = std::modf(-0.0, &f3);
    std::cout << "modf(-0) makes " << f3 << " + " << f2 << '\n';
    f2 = std::modf(-INFINITY, &f3);
    std::cout << "modf(-Inf) makes " << f3 << " + " << f2 << '\n';
}
```

Possible output:

```text
Given the number 123.45 or 0x1.edccccccccccdp+6 in hex,
modf() makes 123 + 0.45
frexp() makes 0.964453 * 2^7
logb()/ilogb() make 1.92891 * 2^6
modf(-0) makes -0 + -0
modf(-Inf) makes -INF + -0
```

### See also

- **trunctruncftruncl (C++11)(C++11)(C++11)** — nearest integer not greater in
  magnitude than the given value (function)

**C documentation for `modf`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/modf*
