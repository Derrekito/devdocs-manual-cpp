# C numeric limits interface

See also `std::numeric_limits` interface.

### Limits of integer types

**Limits of core language integer types**

- **CHAR_BIT** — number of bits in a byte (macro constant)
- **MB_LEN_MAX** — maximum number of bytes in a multibyte character (macro
  constant)
- **CHAR_MIN** — minimum value of char (macro constant)
- **CHAR_MAX** — maximum value of char (macro constant)
- **SCHAR_MINSHRT_MININT_MINLONG_MINLLONG_MIN (C++11)** — minimum value of
  signed char, short, int, long and long long respectively (macro constant)
- **SCHAR_MAXSHRT_MAXINT_MAXLONG_MAXLLONG_MAX (C++11)** — maximum value of
  signed char, short, int, long and long long respectively (macro constant)
- **UCHAR_MAXUSHRT_MAXUINT_MAXULONG_MAXULLONG_MAX (C++11)** — maximum value of
  unsigned char, unsigned short, unsigned int, unsigned long and unsigned long
  long respectively (macro constant)
- **WCHAR_MIN (C++11)** — minimum value of wchar_t (macro constant)
- **WCHAR_MAX (C++11)** — maximum value of wchar_t (macro constant)

**Limits of library type aliases**

- **PTRDIFF_MIN (C++11)** — minimum value of `std::ptrdiff_t` (macro constant)
- **PTRDIFF_MAX (C++11)** — maximum value of `std::ptrdiff_t` (macro constant)
- **SIZE_MAX (C++11)** — maximum value of `std::size_t` (macro constant)
- **SIG_ATOMIC_MIN (C++11)** — minimum value of `std::sig_atomic_t` (macro
  constant)
- **SIG_ATOMIC_MAX (C++11)** — maximum value of `std::sig_atomic_t` (macro
  constant)
- **WINT_MIN (C++11)** — minimum value of `std::wint_t` (macro constant)
- **WINT_MAX (C++11)** — maximum value of `std::wint_t` (macro constant)

#### Notes

The types of these constants, other than `CHAR_BIT` and `MB_LEN_MAX`, are
required to match the results of the integral promotions as applied to objects
of the types they describe: `CHAR_MAX` may have type int or unsigned int, but
never char. Similarly `USHRT_MAX` may not be of an unsigned type: its type may
be int.

A freestanding implementation may lack `std::sig_atomic_t` and/or `std::wint_t`
typedef names, in which case the `SIG_ATOMIC_*` and/or `WINT_*` macros are
correspondingly absent.

#### Example

```cpp
#include <climits>
#include <cstdint>
#include <iomanip>
#include <iostream>

int main()
{
    constexpr int w = 14;
    std::cout << std::left;
#   define COUT(x) std::cout << std::setw(w) << #x << " = " << x << '\n'

    COUT( CHAR_BIT       );
    COUT( MB_LEN_MAX     );
    COUT( CHAR_MIN       );
    COUT( CHAR_MAX       );
    COUT( SCHAR_MIN      );
    COUT( SHRT_MIN       );
    COUT( INT_MIN        );
    COUT( LONG_MIN       );
    COUT( LLONG_MIN      );
    COUT( SCHAR_MAX      );
    COUT( SHRT_MAX       );
    COUT( INT_MAX        );
    COUT( LONG_MAX       );
    COUT( LLONG_MAX      );
    COUT( UCHAR_MAX      );
    COUT( USHRT_MAX      );
    COUT( UINT_MAX       );
    COUT( ULONG_MAX      );
    COUT( ULLONG_MAX     );
    COUT( PTRDIFF_MIN    );
    COUT( PTRDIFF_MAX    );
    COUT( SIZE_MAX       );
    COUT( SIG_ATOMIC_MIN );
    COUT( SIG_ATOMIC_MAX );
    COUT( WCHAR_MIN      );
    COUT( WCHAR_MAX      );
    COUT( WINT_MIN       );
    COUT( WINT_MAX       );
}
```

Possible output:

```text
CHAR_BIT       = 8
MB_LEN_MAX     = 16
CHAR_MIN       = -128
CHAR_MAX       = 127
SCHAR_MIN      = -128
SHRT_MIN       = -32768
INT_MIN        = -2147483648
LONG_MIN       = -9223372036854775808
LLONG_MIN      = -9223372036854775808
SCHAR_MAX      = 127
SHRT_MAX       = 32767
INT_MAX        = 2147483647
LONG_MAX       = 9223372036854775807
LLONG_MAX      = 9223372036854775807
UCHAR_MAX      = 255
USHRT_MAX      = 65535
UINT_MAX       = 4294967295
ULONG_MAX      = 18446744073709551615
ULLONG_MAX     = 18446744073709551615
PTRDIFF_MIN    = -9223372036854775808
PTRDIFF_MAX    = 9223372036854775807
SIZE_MAX       = 18446744073709551615
SIG_ATOMIC_MIN = -2147483648
SIG_ATOMIC_MAX = 2147483647
WCHAR_MIN      = -2147483648
WCHAR_MAX      = 2147483647
WINT_MIN       = 0
WINT_MAX       = 4294967295
```

### Limits of floating-point types

- **FLT_RADIX** — the radix (integer base) used by the representation of all
  three floating-point types (macro constant)
- **DECIMAL_DIG (C++11)** — conversion from long double to decimal with at least
  `DECIMAL_DIG` digits and back to long double is the identity conversion: this
  is the decimal precision required to serialize/deserialize a long double (see
  also `std::numeric_limits::max_digits10`) (macro constant)
- **FLT_DECIMAL_DIGDBL_DECIMAL_DIGLDBL_DECIMAL_DIG (C++17)** — conversion from
  float/double/long double to decimal with at least
  `FLT_DECIMAL_DIG`/`DBL_DECIMAL_DIG`/`LDBL_DECIMAL_DIG` digits and back is the
  identity conversion: this is the decimal precision required to
  serialize/deserialize a floating-point value (see also
  `std::numeric_limits::max_digits10`). Defined to at least `6`, `10`, and `10`
  respectively, or `9` for IEEE float and `17` for IEEE double. (macro constant)
- **FLT_MINDBL_MINLDBL_MIN** — minimum normalized positive value of float,
  double and long double respectively (macro constant)
- **FLT_TRUE_MINDBL_TRUE_MINLDBL_TRUE_MIN (C++17)** — minimum positive value of
  float, double and long double respectively (macro constant)
- **FLT_MAXDBL_MAXLDBL_MAX** — maximum finite value of float, double and long
  double respectively (macro constant)
- **FLT_EPSILONDBL_EPSILONLDBL_EPSILON** — difference between `1.0` and the next
  representable value for float, double and long double respectively (macro
  constant)
- **FLT_DIGDBL_DIGLDBL_DIG** — number of decimal digits that are guaranteed to
  be preserved in text → float/double/long double → text roundtrip without
  change due to rounding or overflow (see `std::numeric_limits::digits10` for
  explanation) (macro constant)
- **FLT_MANT_DIGDBL_MANT_DIGLDBL_MANT_DIG** — number of base `FLT_RADIX` digits
  that can be represented without losing precision for float, double and long
  double respectively (macro constant)
- **FLT_MIN_EXPDBL_MIN_EXPLDBL_MIN_EXP** — minimum negative integer such that
  `FLT_RADIX` raised by power one less than that integer is a normalized float,
  double and long double respectively (macro constant)
- **FLT_MIN_10_EXPDBL_MIN_10_EXPLDBL_MIN_10_EXP** — minimum negative integer
  such that `10` raised to that power is a normalized float, double and long
  double respectively (macro constant)
- **FLT_MAX_EXPDBL_MAX_EXPLDBL_MAX_EXP** — maximum positive integer such that
  `FLT_RADIX` raised by power one less than that integer is a representable
  finite float, double and long double respectively (macro constant)
- **FLT_MAX_10_EXPDBL_MAX_10_EXPLDBL_MAX_10_EXP** — maximum positive integer
  such that `10` raised to that power is a representable finite float, double
  and long double respectively (macro constant)
- **FLT_ROUNDS** — default rounding mode of floating-point arithmetic (macro
  constant)
- **FLT_EVAL_METHOD (C++11)** — specifies in what precision all arithmetic
  operations are done (macro constant)
- **FLT_HAS_SUBNORMDBL_HAS_SUBNORMLDBL_HAS_SUBNORM (C++17)** — specifies whether
  the type supports subnormal (denormal) numbers: `-1` – indeterminable, `​0​` –
  absent, `1` – present (macro constant)

#### Example

```cpp
#include <cfloat>
#include <iomanip>
#include <iostream>

int main()
{
    int w = 16;
    std::cout << std::left; // std::cout << std::setprecision(53);
#   define COUT(x) std::cout << std::setw(w) << #x << " = " << x << '\n'

    COUT( FLT_RADIX        );
    COUT( DECIMAL_DIG      );
    COUT( FLT_DECIMAL_DIG  );
    COUT( DBL_DECIMAL_DIG  );
    COUT( LDBL_DECIMAL_DIG );
    COUT( FLT_MIN          );
    COUT( DBL_MIN          );
    COUT( LDBL_MIN         );
    COUT( FLT_TRUE_MIN     );
    COUT( DBL_TRUE_MIN     );
    COUT( LDBL_TRUE_MIN    );
    COUT( FLT_MAX          );
    COUT( DBL_MAX          );
    COUT( LDBL_MAX         );
    COUT( FLT_EPSILON      );
    COUT( DBL_EPSILON      );
    COUT( LDBL_EPSILON     );
    COUT( FLT_DIG          );
    COUT( DBL_DIG          );
    COUT( LDBL_DIG         );
    COUT( FLT_MANT_DIG     );
    COUT( DBL_MANT_DIG     );
    COUT( LDBL_MANT_DIG    );
    COUT( FLT_MIN_EXP      );
    COUT( DBL_MIN_EXP      );
    COUT( LDBL_MIN_EXP     );
    COUT( FLT_MIN_10_EXP   );
    COUT( DBL_MIN_10_EXP   );
    COUT( LDBL_MIN_10_EXP  );
    COUT( FLT_MAX_EXP      );
    COUT( DBL_MAX_EXP      );
    COUT( LDBL_MAX_EXP     );
    COUT( FLT_MAX_10_EXP   );
    COUT( DBL_MAX_10_EXP   );
    COUT( LDBL_MAX_10_EXP  );
    COUT( FLT_ROUNDS       );
    COUT( FLT_EVAL_METHOD  );
    COUT( FLT_HAS_SUBNORM  );
    COUT( DBL_HAS_SUBNORM  );
    COUT( LDBL_HAS_SUBNORM );
}
```

Possible output:

```text
FLT_RADIX        = 2
DECIMAL_DIG      = 21
FLT_DECIMAL_DIG  = 9
DBL_DECIMAL_DIG  = 17
LDBL_DECIMAL_DIG = 21
FLT_MIN          = 1.17549e-38
DBL_MIN          = 2.22507e-308
LDBL_MIN         = 3.3621e-4932
FLT_TRUE_MIN     = 1.4013e-45
DBL_TRUE_MIN     = 4.94066e-324
LDBL_TRUE_MIN    = 3.6452e-4951
FLT_MAX          = 3.40282e+38
DBL_MAX          = 1.79769e+308
LDBL_MAX         = 1.18973e+4932
FLT_EPSILON      = 1.19209e-07
DBL_EPSILON      = 2.22045e-16
LDBL_EPSILON     = 1.0842e-19
FLT_DIG          = 6
DBL_DIG          = 15
LDBL_DIG         = 18
FLT_MANT_DIG     = 24
DBL_MANT_DIG     = 53
LDBL_MANT_DIG    = 64
FLT_MIN_EXP      = -125
DBL_MIN_EXP      = -1021
LDBL_MIN_EXP     = -16381
FLT_MIN_10_EXP   = -37
DBL_MIN_10_EXP   = -307
LDBL_MIN_10_EXP  = -4931
FLT_MAX_EXP      = 128
DBL_MAX_EXP      = 1024
LDBL_MAX_EXP     = 16384
FLT_MAX_10_EXP   = 38
DBL_MAX_10_EXP   = 308
LDBL_MAX_10_EXP  = 4932
FLT_ROUNDS       = 1
FLT_EVAL_METHOD  = 0
FLT_HAS_SUBNORM  = 1
DBL_HAS_SUBNORM  = 1
LDBL_HAS_SUBNORM = 1
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 416 | C++98 | it was unclear whether the types of the macros in
      `<climits>` are guaranteed to match the type to which they refer (C++
      refers to C, and C says no) | clarified as not guaranteed

### See also

- Fixed width integer types
- Arithmetic types
- C++ type system overview
- Type support (basic types, RTTI, type traits)

**C documentation for Numeric limits**

---
*Source: https://en.cppreference.com/w/cpp/types/climits*
