# std::atan, std::atanf, std::atanl

```cpp
float       atan ( float num );
double      atan ( double num );
long double atan ( long double num );  // (until C++23)
/* floating-point-type */
            atan ( /* floating-point-type */ num );  // (since C++23) (constexpr since C++26)
float       atanf( float num );  // (2) (since C++11) (constexpr since C++26)
long double atanl( long double num );  // (3) (since C++11) (constexpr since C++26)
Additional overloads (since C++11)
template< class Integer >
double      atan ( Integer num );  // (A) (constexpr since C++26)
```

1-3) Computes the principal value of the arc tangent of `num`. The library
   provides overloads of `std::atan` for all cv-unqualified floating-point types
   as the type of the parameter.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
double.
*(since C++11)*

### Parameters

- **num** — floating-point or integer value

### Return value

If no errors occur, the arc tangent of `num` (arctan(num)) in the range [- π/2 ,
+π/2] radians, is returned.

If a range error occurs due to underflow, the correct result (after rounding) is
returned.

### Error handling

Errors are reported as specified in `math_errhandling`.

If the implementation supports IEEE floating-point arithmetic (IEC 60559),

- If the argument is ±0, it is returned unmodified.
- If the argument is +∞, +π/2 is returned.
- If the argument is -∞, -π/2 is returned.
- If the argument is NaN, NaN is returned.

### Notes

POSIX specifies that in case of underflow, `num` is returned unmodified, and if
that is not supported, an implementation-defined value no greater than
`DBL_MIN`, `FLT_MIN`, and `LDBL_MIN` is returned.

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their argument `num` of integer
type, `std::atan(num)` has the same effect as
`std::atan(static_cast<double>(num))`.

### Example

```cpp
#include <cmath>
#include <iostream>

int main()
{
    std::cout << "atan(1) = " << std::atan(1) << '\n'
              << "4*atan(1) = " << 4 * std::atan(1) << '\n';

    // special values
    std::cout << "atan(Inf) = " << std::atan(INFINITY) << '\n'
              << "2*atan(Inf) = " << 2 * std::atan(INFINITY) << '\n'
              << "atan(-0.0) = " << std::atan(-0.0) << '\n'
              << "atan(+0.0) = " << std::atan(0) << '\n';
}
```

Output:

```text
atan(1) = 0.785398
4*atan(1) = 3.14159
atan(Inf) = 1.5708
2*atan(Inf) = 3.14159
atan(-0.0) = -0
atan(+0.0) = 0
```

### See also

- **asinasinfasinl (C++11)(C++11)** — computes arc sine
  (\({\small\arcsin{x}}\)arcsin(x)) (function)
- **acosacosfacosl (C++11)(C++11)** — computes arc cosine
  (\({\small\arccos{x}}\)arccos(x)) (function)
- **atan2atan2fatan2l (C++11)(C++11)** — arc tangent, using signs to determine
  quadrants (function)
- **tantanftanl (C++11)(C++11)** — computes tangent (\({\small\tan{x}}\)tan(x))
  (function)
- **atan(std::complex) (C++11)** — computes arc tangent of a complex number
  (\({\small\arctan{z}}\)arctan(z)) (function template)
- **atan(std::valarray)** — applies the function `std::atan` to each element of
  valarray (function template)

**C documentation for `atan`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/atan*
