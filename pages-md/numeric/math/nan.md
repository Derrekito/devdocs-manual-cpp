# std::nan, std::nanf, std::nanl

```cpp
float       nanf( const char* arg );  // (1) (since C++11)
double      nan ( const char* arg );  // (2) (since C++11)
long double nanl( const char* arg );  // (3) (since C++11)
```

Converts the character string `arg` into the corresponding quiet NaN value, as
if by calling `std::strtof`, `std::strtod`, or `std::strtold`, respectively.

1) The call `std::nanf("n-char-sequence")`, where n-char-sequence is a sequence
   of digits, ASCII letters, and underscores, is equivalent to the call
   `std::strtof("NAN(n-char-sequence)", (char**)nullptr);`.

The call `std::nanf("")` is equivalent to the call `std::strtof("NAN()",
   (char**)nullptr);`.

The call `std::nanf("string")`, where string is neither an n-char-sequence nor
   an empty string, is equivalent to the call `std::strtof("NAN",
   (char**)nullptr);`.

2) Same as (1), but calls `std::strtod` instead of `std::strtof`.

3) Same as (1), but calls `std::strtold` instead of `std::strtof`.

### Parameters

- **arg** — narrow character string identifying the contents of a NaN

### Return value

The quiet NaN value that corresponds to the identifying string `arg` or zero if
the implementation does not support quiet NaNs.

If the implementation supports IEEE floating-point arithmetic (IEC 60559), it
also supports quiet NaNs.

### Error handling

This function is not subject to any of the error conditions specified in
`math_errhandling`.

### Example

```cpp
#include <cmath>
#include <cstdint>
#include <cstring>
#include <iostream>

int main()
{
    double f1 = std::nan("1");
    std::uint64_t f1n; std::memcpy(&f1n, &f1, sizeof f1);
    std::cout << "nan(\"1\") = " << f1 << " (" << std::hex << f1n << ")\n";

    double f2 = std::nan("2");
    std::uint64_t f2n; std::memcpy(&f2n, &f2, sizeof f2);
    std::cout << "nan(\"2\") = " << f2 << " (" << std::hex << f2n << ")\n";
}
```

Possible output:

```text
nan("1") = nan (7ff0000000000001)
nan("2") = nan (7ff0000000000002)
```

### See also

- **isnan (C++11)** — checks if the given number is NaN (function)
- **NAN (C++11)** — evaluates to a quiet NaN of type float (macro constant)
- **has_quiet_NaN [static]** — identifies floating-point types that can
  represent the special value "quiet not-a-number" (NaN) (public static member
  constant of `std::numeric_limits<T>`)
- **has_signaling_NaN [static]** — identifies floating-point types that can
  represent the special value "signaling not-a-number" (NaN) (public static
  member constant of `std::numeric_limits<T>`)
- **quiet_NaN [static]** — returns a quiet NaN value of the given floating-point
  type (public static member function of `std::numeric_limits<T>`)
- **signaling_NaN [static]** — returns a signaling NaN value of the given
  floating-point type (public static member function of
  `std::numeric_limits<T>`)

**C documentation for `nanf, nan, nanl`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/nan*
