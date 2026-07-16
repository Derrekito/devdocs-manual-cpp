# Fixed width integer types (since C++11)

### Types

- **int8_tint16_tint32_tint64_t (optional)** — signed integer type with width of
  exactly 8, 16, 32 and 64 bits respectively with no padding bits and using 2's
  complement for negative values (provided if and only if the implementation
  directly supports the type) (typedef)
- **int_fast8_tint_fast16_tint_fast32_tint_fast64_t** — fastest signed integer
  type with width of at least 8, 16, 32 and 64 bits respectively (typedef)
- **int_least8_tint_least16_tint_least32_tint_least64_t** — smallest signed
  integer type with width of at least 8, 16, 32 and 64 bits respectively
  (typedef)
- **intmax_t** — maximum-width signed integer type (typedef)
- **intptr_t (optional)** — signed integer type capable of holding a pointer to
  void (typedef)
- **uint8_tuint16_tuint32_tuint64_t (optional)** — unsigned integer type with
  width of exactly 8, 16, 32 and 64 bits respectively (provided if and only if
  the implementation directly supports the type) (typedef)
- **uint_fast8_tuint_fast16_tuint_fast32_tuint_fast64_t** — fastest unsigned
  integer type with width of at least 8, 16, 32 and 64 bits respectively
  (typedef)
- **uint_least8_tuint_least16_tuint_least32_tuint_least64_t** — smallest
  unsigned integer type with width of at least 8, 16, 32 and 64 bits
  respectively (typedef)
- **uintmax_t** — maximum-width unsigned integer type (typedef)
- **uintptr_t (optional)** — unsigned integer type capable of holding a pointer
  to void (typedef)

The implementation may define typedef names `intN_t`, `int_fastN_t`,
`int_leastN_t`, `uintN_t`, `uint_fastN_t`, and `uint_leastN_t` when *N* is not
8, 16, 32 or 64. Typedef names of the form `intN_t` may only be defined if the
implementation supports an integer type of that width with no padding. Thus,
`std::uint24_t` denotes an unsigned integer type with a width of exactly 24
bits.

Each of the macros listed in below is defined if and only if the implementation
defines the corresponding typedef name. The macros `INTN_C` and `UINTN_C`
correspond to the typedef names `int_leastN_t` and `uint_leastN_t`,
respectively.

### Macro constants

**Signed integers : minimum value**

- **INT8_MININT16_MININT32_MININT64_MIN (optional)** — minimum value of
  `std::int8_t`, `std::int16_t`, `std::int32_t` and `std::int64_t` respectively
  (macro constant)
- **INT_FAST8_MININT_FAST16_MININT_FAST32_MININT_FAST64_MIN** — minimum value of
  `std::int_fast8_t`, `std::int_fast16_t`, `std::int_fast32_t` and
  `std::int_fast64_t` respectively (macro constant)
- **INT_LEAST8_MININT_LEAST16_MININT_LEAST32_MININT_LEAST64_MIN** — minimum
  value of `std::int_least8_t`, `std::int_least16_t`, `std::int_least32_t` and
  `std::int_least64_t` respectively (macro constant)
- **INTPTR_MIN (optional)** — minimum value of `std::intptr_t` (macro constant)
- **INTMAX_MIN** — minimum value of `std::intmax_t` (macro constant)

**Signed integers : maximum value**

- **INT8_MAXINT16_MAXINT32_MAXINT64_MAX (optional)** — maximum value of
  `std::int8_t`, `std::int16_t`, `std::int32_t` and `std::int64_t` respectively
  (macro constant)
- **INT_FAST8_MAXINT_FAST16_MAXINT_FAST32_MAXINT_FAST64_MAX** — maximum value of
  `std::int_fast8_t`, `std::int_fast16_t`, `std::int_fast32_t` and
  `std::int_fast64_t` respectively (macro constant)
- **INT_LEAST8_MAXINT_LEAST16_MAXINT_LEAST32_MAXINT_LEAST64_MAX** — maximum
  value of `std::int_least8_t`, `std::int_least16_t`, `std::int_least32_t` and
  `std::int_least64_t` respectively (macro constant)
- **INTPTR_MAX (optional)** — maximum value of `std::intptr_t` (macro constant)
- **INTMAX_MAX** — maximum value of `std::intmax_t` (macro constant)

**Unsigned integers : maximum value**

- **UINT8_MAXUINT16_MAXUINT32_MAXUINT64_MAX (optional)** — maximum value of
  `std::uint8_t`, `std::uint16_t`, `std::uint32_t` and `std::uint64_t`
  respectively (macro constant)
- **UINT_FAST8_MAXUINT_FAST16_MAXUINT_FAST32_MAXUINT_FAST64_MAX** — maximum
  value of `std::uint_fast8_t`, `std::uint_fast16_t`, `std::uint_fast32_t` and
  `std::uint_fast64_t` respectively (macro constant)
- **UINT_LEAST8_MAXUINT_LEAST16_MAXUINT_LEAST32_MAXUINT_LEAST64_MAX** — maximum
  value of `std::uint_least8_t`, `std::uint_least16_t`, `std::uint_least32_t`
  and `std::uint_least64_t` respectively (macro constant)
- **UINTPTR_MAX (optional)** — maximum value of `std::uintptr_t` (macro
  constant)
- **UINTMAX_MAX** — maximum value of `std::uintmax_t` (macro constant)

### Function macros for minimum-width integer constants

- **INT8_CINT16_CINT32_CINT64_C** — expands to an integer constant expression
  having the value specified by its argument and whose type is the promoted type
  of `std::int_least8_t`, `std::int_least16_t`, `std::int_least32_t` and
  `std::int_least64_t` respectively (function macro)
- **INTMAX_C** — expands to an integer constant expression having the value
  specified by its argument and the type `std::intmax_t` (function macro)
- **UINT8_CUINT16_CUINT32_CUINT64_C** — expands to an integer constant
  expression having the value specified by its argument and whose type is the
  promoted type of `std::uint_least8_t`, `std::uint_least16_t`,
  `std::uint_least32_t` and `std::uint_least64_t` respectively (function macro)
- **UINTMAX_C** — expands to an integer constant expression having the value
  specified by its argument and the type `std::uintmax_t` (function macro)

```cpp
#include <cstdint>
UINT64_C(0x123) // expands to a literal of type uint_least64_t and value 0x123
```

### Format macro constants

#### Format constants for the `std::fprintf` family of functions

Each of the `PRI` macros listed here is defined if and only if the
implementation defines the corresponding typedef name.

  Equivalent for int or unsigned int | Description | Macros for data types
  `std::int`**x**`_t` | `std::int_least`**x**`_t` | `std::int_fast`**x**`_t` |
      `std::intmax_t` | `std::intptr_t`
  `d` | output of a signed decimal integer value | PRId**x** | PRIdLEAST**x** |
      PRIdFAST**x** | PRIdMAX | PRIdPTR
  `i` | PRIi**x** | PRIiLEAST**x** | PRIiFAST**x** | PRIiMAX | PRIiPTR
  `u` | output of an unsigned decimal integer value | PRIu**x** | PRIuLEAST**x**
      | PRIuFAST**x** | PRIuMAX | PRIuPTR
  `o` | output of an unsigned octal integer value | PRIo**x** | PRIoLEAST**x** |
      PRIoFAST**x** | PRIoMAX | PRIoPTR
  `x` | output of an unsigned lowercase hexadecimal integer value | PRIx**x** |
      PRIxLEAST**x** | PRIxFAST**x** | PRIxMAX | PRIxPTR
  `X` | output of an unsigned uppercase hexadecimal integer value | PRIX**x** |
      PRIXLEAST**x** | PRIXFAST**x** | PRIXMAX | PRIXPTR

#### Format constants for the `std::fscanf` family of functions

Each of the `SCN` macros listed in here is defined if and only if the
implementation defines the corresponding typedef name and has a suitable
`std::fscanf` length modifier for the type.

  Equivalent for int or unsigned int | Description | Macros for data types
  `std::int`**x**`_t` | `std::int_least`**x**`_t` | `std::int_fast`**x**`_t` |
      `std::intmax_t` | `std::intptr_t`
  `d` | input of a signed decimal integer value | SCNd**x** | SCNdLEAST**x** |
      SCNdFAST**x** | SCNdMAX | SCNdPTR
  `i` | input of a signed integer value | SCNi**x** | SCNiLEAST**x** |
      SCNiFAST**x** | SCNiMAX | SCNiPTR
  `u` | input of an unsigned decimal integer value | SCNu**x** | SCNuLEAST**x**
      | SCNuFAST**x** | SCNuMAX | SCNuPTR
  `o` | input of an unsigned octal integer value | SCNo**x** | SCNoLEAST**x** |
      SCNoFAST**x** | SCNoMAX | SCNoPTR
  `x` | input of an unsigned hexadecimal integer value | SCNx**x** |
      SCNxLEAST**x** | SCNxFAST**x** | SCNxMAX | SCNxPTR

### Notes

Because C++ interprets a character immediately following a string literal as a
user-defined string literal, C code such as `printf("%"PRId64"\n",n);` is
invalid C++ and requires a space before `PRId64`.

The C99 standard suggests that C++ implementations should not define the above
limit, constant, or format macros unless the macros `__STDC_LIMIT_MACROS`,
`__STDC_CONSTANT_MACROS` or `__STDC_FORMAT_MACROS` (respectively) are defined
before including the relevant C header (`stdint.h` or `inttypes.h`). This
recommendation was not adopted by any C++ standard and was removed in C11.
However, some implementations (such as glibc 2.17) try to apply this rule, and
it may be necessary to define the `__STDC` macros; C++ compilers may try to work
around this by automatically defining them in some circumstances.

`std::int8_t` may be signed char and `std::uint8_t` may be unsigned char, but
neither can be char regardless of its signedness (because char is not considered
a "signed integer type" or "unsigned integer type").

### Example

See also a note regarding spaces before format macros used in this example.

```cpp
#include <cinttypes>
#include <cstdio>

int main()
{
    std::printf("%zu\n", sizeof(std::int64_t));
    std::printf("%s\n", PRId64);
    std::printf("%+" PRId64 "\n", INT64_MIN);
    std::printf("%+" PRId64 "\n", INT64_MAX);

    std::int64_t n = 7;
    std::printf("%+" PRId64 "\n", n);
}
```

Possible output:

```text
8
lld
-9223372036854775808
+9223372036854775807
+7
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2820 | C++11 | the requirements for optional typedef names and macros were
      inconsistent with C | made consistent

### References

- C++23 standard (ISO/IEC 14882:2023):
- C++20 standard (ISO/IEC 14882:2020):
- C++17 standard (ISO/IEC 14882:2017):
- C++14 standard (ISO/IEC 14882:2014):
- C++11 standard (ISO/IEC 14882:2011):

### See also

- Fundamental types

**C documentation for Fixed width integer types**

---
*Source: https://en.cppreference.com/w/cpp/types/integer*
