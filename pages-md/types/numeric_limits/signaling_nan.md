# std::numeric_limits<T>::signaling_NaN

```cpp
static T signaling_NaN() throw();  // (until C++11)
static constexpr T signaling_NaN() noexcept;  // (since C++11)
```

Returns the special value "signaling not-a-number", as represented by the
floating-point type `T`. Only meaningful if
`std::numeric_limits<T>::has_signaling_NaN == true`. In IEEE 754, the most
common binary representation of floating-point numbers, any value with all bits
of the exponent set and at least one bit of the fraction set represents a NaN.
It is implementation-defined which values of the fraction represent quiet or
signaling NaNs, and whether the sign bit is meaningful.

### Return value

- **/* non-specialized */** — `T()`
- **bool** — `false`
- **char** — `​0​`
- **signed char** — `​0​`
- **unsigned char** — `​0​`
- **wchar_t** — `​0​`
- **char8_t (since C++20)** — `​0​`
- **char16_t (since C++11)** — `​0​`
- **char32_t (since C++11)** — `​0​`
- **short** — `​0​`
- **unsigned short** — `​0​`
- **int** — `​0​`
- **unsigned int** — `​0​`
- **long** — `​0​`
- **unsigned long** — `​0​`
- **long long (since C++11)** — `​0​`
- **unsigned long long (since C++11)** — `​0​`
- **float** — implementation-defined (may be `FLT_SNAN`)
- **double** — implementation-defined (may be `DBL_SNAN`)
- **long double** — implementation-defined (may be `LDBL_SNAN`)

### Notes

A NaN never compares equal to itself. Copying a NaN is not required, by
IEEE-754, to preserve its bit representation (sign and payload), though most
implementation do.

When a signaling NaN is used as an argument to an arithmetic expression, the
appropriate floating-point exception may be raised and the NaN is "quieted",
that is, the expression returns a quiet NaN.

### Example

Demonstrates the use of a signaling NaN to raise a floating-point exception:

```cpp
#include <cfenv>
#include <iostream>
#include <limits>

#pragma STDC_FENV_ACCESS on

void show_fe_exceptions()
{
    int n = std::fetestexcept(FE_ALL_EXCEPT);

    if (n & FE_INVALID)
        std::cout << "FE_INVALID is raised\n";
    else if (n == 0)
        std::cout << "no exceptions are raised\n";

    std::feclearexcept(FE_ALL_EXCEPT);
}

int main()
{
    double snan = std::numeric_limits<double>::signaling_NaN();
    std::cout << "After sNaN was obtained, ";
    show_fe_exceptions();

    double qnan = snan * 2.0;
    std::cout << "After sNaN was multiplied by 2, ";
    show_fe_exceptions();

    double qnan2 = qnan * 2.0;
    std::cout << "After the quieted NaN was multiplied by 2, ";
    show_fe_exceptions();

    std::cout << "The result is " << qnan2 << '\n';
}
```

Output:

```text
After sNaN was obtained, no exceptions are raised
After sNaN was multiplied by 2, FE_INVALID is raised
After the quieted NaN was multiplied by 2, no exceptions are raised
The result is nan
```

### See also

- **has_signaling_NaN [static]** — identifies floating-point types that can
  represent the special value "signaling not-a-number" (NaN) (public static
  member constant)
- **quiet_NaN [static]** — returns a quiet NaN value of the given floating-point
  type (public static member function)
- **isnan (C++11)** — checks if the given number is NaN (function)

---
*Source: https://en.cppreference.com/w/cpp/types/numeric_limits/signaling_NaN*
