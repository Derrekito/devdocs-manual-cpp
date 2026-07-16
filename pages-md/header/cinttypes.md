# Standard library header <cinttypes> (C++11)

This header was originally in the C standard library as `<inttypes.h>`.

**Includes**

- **<cstdint> (C++11)** — Fixed-width integer types and limits of other types

**Types**

- **imaxdiv_t (C++11)** — structure type, returned by `std::imaxdiv` (typedef)

**Functions**

- **abs(std::intmax_t)imaxabs (C++11)(C++11)** — computes absolute value of an
  integral value (\(\small{|x|}\)|x|) (function)
- **div(std::intmax_t)imaxdiv (C++11)(C++11)** — computes quotient and remainder
  of integer division (function)
- **strtoimaxstrtoumax (C++11)(C++11)** — converts a byte string to
  `std::intmax_t` or `std::uintmax_t` (function)
- **wcstoimaxwcstoumax (C++11)(C++11)** — converts a wide string to
  `std::intmax_t` or `std::uintmax_t` (function)

**Macros**

**Format constants for the `std::fprintf` family of functions**

-
  **PRId8PRId16PRId32PRId64PRIdLEAST8PRIdLEAST16PRIdLEAST32PRIdLEAST64PRIdFAST8PRIdFAST16PRIdFAST32PRIdFAST64PRIdMAXPRIdPTR
  (C++11)** — format conversion specifier to output a signed decimal integer
  value of type `std::int8_t`, `std::int16_t`, `std::int32_t`, `std::int64_t`,
  `std::int_least8_t`, `std::int_least16_t`, `std::int_least32_t`,
  `std::int_least64_t`, `std::int_fast8_t`, `std::int_fast16_t`,
  `std::int_fast32_t`, `std::int_fast64_t`, `std::intmax_t`, `std::intptr_t`
  respectively, equivalent to `d` for int (macro constant)
-
  **PRIi8PRIi16PRIi32PRIi64PRIiLEAST8PRIiLEAST16PRIiLEAST32PRIiLEAST64PRIiFAST8PRIiFAST16PRIiFAST32PRIiFAST64PRIiMAXPRIiPTR
  (C++11)** — format conversion specifier to output a signed decimal integer
  value of type `std::int8_t`, `std::int16_t`, `std::int32_t`, `std::int64_t`,
  `std::int_least8_t`, `std::int_least16_t`, `std::int_least32_t`,
  `std::int_least64_t`, `std::int_fast8_t`, `std::int_fast16_t`,
  `std::int_fast32_t`, `std::int_fast64_t`, `std::intmax_t`, `std::intptr_t`
  respectively, equivalent to `i` for int (macro constant)
-
  **PRIu8PRIu16PRIu32PRIu64PRIuLEAST8PRIuLEAST16PRIuLEAST32PRIuLEAST64PRIuFAST8PRIuFAST16PRIuFAST32PRIuFAST64PRIuMAXPRIuPTR
  (C++11)** — format conversion specifier to output an unsigned decimal integer
  value of type `std::uint8_t`, `std::uint16_t`, `std::uint32_t`,
  `std::uint64_t`, `std::uint_least8_t`, `std::uint_least16_t`,
  `std::uint_least32_t`, `std::uint_least64_t`, `std::uint_fast8_t`,
  `std::uint_fast16_t`, `std::uint_fast32_t`, `std::uint_fast64_t`,
  `std::uintmax_t`, `std::uintptr_t` respectively, equivalent to `u` for
  unsigned int (macro constant)
-
  **PRIo8PRIo16PRIo32PRIo64PRIoLEAST8PRIoLEAST16PRIoLEAST32PRIoLEAST64PRIoFAST8PRIoFAST16PRIoFAST32PRIoFAST64PRIoMAXPRIoPTR
  (C++11)** — format conversion specifier to output an unsigned octal integer
  value of type `std::uint8_t`, `std::uint16_t`, `std::uint32_t`,
  `std::uint64_t`, `std::uint_least8_t`, `std::uint_least16_t`,
  `std::uint_least32_t`, `std::uint_least64_t`, `std::uint_fast8_t`,
  `std::uint_fast16_t`, `std::uint_fast32_t`, `std::uint_fast64_t`,
  `std::uintmax_t`, `std::uintptr_t` respectively, equivalent to `o` for
  unsigned int (macro constant)
-
  **PRIx8PRIx16PRIx32PRIx64PRIxLEAST8PRIxLEAST16PRIxLEAST32PRIxLEAST64PRIxFAST8PRIxFAST16PRIxFAST32PRIxFAST64PRIxMAXPRIxPTR
  (C++11)** — format conversion specifier to output an unsigned lowercase
  hexadecimal integer value of type `std::uint8_t`, `std::uint16_t`,
  `std::uint32_t`, `std::uint64_t`, `std::uint_least8_t`, `std::uint_least16_t`,
  `std::uint_least32_t`, `std::uint_least64_t`, `std::uint_fast8_t`,
  `std::uint_fast16_t`, `std::uint_fast32_t`, `std::uint_fast64_t`,
  `std::uintmax_t`, `std::uintptr_t` respectively, equivalent to `x` for
  unsigned int (macro constant)
-
  **PRIX8PRIX16PRIX32PRIX64PRIXLEAST8PRIXLEAST16PRIXLEAST32PRIXLEAST64PRIXFAST8PRIXFAST16PRIXFAST32PRIXFAST64PRIXMAXPRIXPTR
  (C++11)** — format conversion specifier to output an unsigned uppercase
  hexadecimal integer value of type `std::uint8_t`, `std::uint16_t`,
  `std::uint32_t`, `std::uint64_t`, `std::uint_least8_t`, `std::uint_least16_t`,
  `std::uint_least32_t`, `std::uint_least64_t`, `std::uint_fast8_t`,
  `std::uint_fast16_t`, `std::uint_fast32_t`, `std::uint_fast64_t`,
  `std::uintmax_t`, `std::uintptr_t` respectively, equivalent to `X` for
  unsigned int (macro constant)

**Format constants for the `std::fscanf` family of functions**

-
  **SCNd8SCNd16SCNd32SCNd64SCNdLEAST8SCNdLEAST16SCNdLEAST32SCNdLEAST64SCNdFAST8SCNdFAST16SCNdFAST32SCNdFAST64SCNdMAXSCNdPTR
  (C++11)** — format conversion specifier to input a signed decimal integer
  value of type `std::int8_t`, `std::int16_t`, `std::int32_t`, `std::int64_t`,
  `std::int_least8_t`, `std::int_least16_t`, `std::int_least32_t`,
  `std::int_least64_t`, `std::int_fast8_t`, `std::int_fast16_t`,
  `std::int_fast32_t`, `std::int_fast64_t`, `std::intmax_t`, `std::intptr_t`
  respectively, equivalent to `d` for int (macro constant)
-
  **SCNi8SCNi16SCNi32SCNi64SCNiLEAST8SCNiLEAST16SCNiLEAST32SCNiLEAST64SCNiFAST8SCNiFAST16SCNiFAST32SCNiFAST64SCNiMAXSCNiPTR
  (C++11)** — format conversion specifier to input a signed
  decimal/octal/hexadecimal integer value of type `std::int8_t`, `std::int16_t`,
  `std::int32_t`, `std::int64_t`, `std::int_least8_t`, `std::int_least16_t`,
  `std::int_least32_t`, `std::int_least64_t`, `std::int_fast8_t`,
  `std::int_fast16_t`, `std::int_fast32_t`, `std::int_fast64_t`,
  `std::intmax_t`, `std::intptr_t` respectively, equivalent to `i` for int
  (macro constant)
-
  **SCNu8SCNu16SCNu32SCNu64SCNuLEAST8SCNuLEAST16SCNuLEAST32SCNuLEAST64SCNuFAST8SCNuFAST16SCNuFAST32SCNuFAST64SCNuMAXSCNuPTR
  (C++11)** — format conversion specifier to input an unsigned decimal integer
  value of type `std::uint8_t`, `std::uint16_t`, `std::uint32_t`,
  `std::uint64_t`, `std::uint_least8_t`, `std::uint_least16_t`,
  `std::uint_least32_t`, `std::uint_least64_t`, `std::uint_fast8_t`,
  `std::uint_fast16_t`, `std::uint_fast32_t`, `std::uint_fast64_t`,
  `std::uintmax_t`, `std::uintptr_t` respectively, equivalent to `u` for
  unsigned int (macro constant)
-
  **SCNo8SCNo16SCNo32SCNo64SCNoLEAST8SCNoLEAST16SCNoLEAST32SCNoLEAST64SCNoFAST8SCNoFAST16SCNoFAST32SCNoFAST64SCNoMAXSCNoPTR
  (C++11)** — format conversion specifier to input an unsigned octal integer
  value of type `std::uint8_t`, `std::uint16_t`, `std::uint32_t`,
  `std::uint64_t`, `std::uint_least8_t`, `std::uint_least16_t`,
  `std::uint_least32_t`, `std::uint_least64_t`, `std::uint_fast8_t`,
  `std::uint_fast16_t`, `std::uint_fast32_t`, `std::uint_fast64_t`,
  `std::uintmax_t`, `std::uintptr_t` respectively, equivalent to `o` for
  unsigned int (macro constant)
-
  **SCNx8SCNx16SCNx32SCNx64SCNxLEAST8SCNxLEAST16SCNxLEAST32SCNxLEAST64SCNxFAST8SCNxFAST16SCNxFAST32SCNxFAST64SCNxMAXSCNxPTR
  (C++11)** — format conversion specifier to input an unsigned hexadecimal
  integer value of type `std::uint8_t`, `std::uint16_t`, `std::uint32_t`,
  `std::uint64_t`, `std::uint_least8_t`, `std::uint_least16_t`,
  `std::uint_least32_t`, `std::uint_least64_t`, `std::uint_fast8_t`,
  `std::uint_fast16_t`, `std::uint_fast32_t`, `std::uint_fast64_t`,
  `std::uintmax_t`, `std::uintptr_t` respectively, equivalent to `x` for
  unsigned int (macro constant)

### Synopsis

```cpp
#include <cstdint>

namespace std
{
    using imaxdiv_t = /* see description */;

    constexpr intmax_t imaxabs(intmax_t j);
    constexpr imaxdiv_t imaxdiv(intmax_t numer, intmax_t denom);
    intmax_t strtoimax(const char* nptr, char** endptr, int base);
    uintmax_t strtoumax(const char* nptr, char** endptr, int base);
    intmax_t wcstoimax(const wchar_t* nptr, wchar_t** endptr, int base);
    uintmax_t wcstoumax(const wchar_t* nptr, wchar_t** endptr, int base);

    constexpr intmax_t abs(intmax_t);            // optional, see description
    constexpr imaxdiv_t div(intmax_t, intmax_t); // optional, see description
}

#define PRIdN /* see description */
#define PRIiN /* see description */
#define PRIoN /* see description */
#define PRIuN /* see description */
#define PRIxN /* see description */
#define PRIXN /* see description */
#define SCNdN /* see description */
#define SCNiN /* see description */
#define SCNoN /* see description */
#define SCNuN /* see description */
#define SCNxN /* see description */
#define PRIdLEASTN /* see description */
#define PRIiLEASTN /* see description */
#define PRIoLEASTN /* see description */
#define PRIuLEASTN /* see description */
#define PRIxLEASTN /* see description */
#define PRIXLEASTN /* see description */
#define SCNdLEASTN /* see description */
#define SCNiLEASTN /* see description */
#define SCNoLEASTN /* see description */
#define SCNuLEASTN /* see description */
#define SCNxLEASTN /* see description */
#define PRIdFASTN /* see description */
#define PRIiFASTN /* see description */
#define PRIoFASTN /* see description */
#define PRIuFASTN /* see description */
#define PRIxFASTN /* see description */
#define PRIXFASTN /* see description */
#define SCNdFASTN /* see description */
#define SCNiFASTN /* see description */
#define SCNoFASTN /* see description */
#define SCNuFASTN /* see description */
#define SCNxFASTN /* see description */
#define PRIdMAX /* see description */
#define PRIiMAX /* see description */
#define PRIoMAX /* see description */
#define PRIuMAX /* see description */
#define PRIxMAX /* see description */
#define PRIXMAX /* see description */
#define SCNdMAX /* see description */
#define SCNiMAX /* see description */
#define SCNoMAX /* see description */
#define SCNuMAX /* see description */
#define SCNxMAX /* see description */
#define PRIdPTR /* see description */
#define PRIiPTR /* see description */
#define PRIoPTR /* see description */
#define PRIuPTR /* see description */
#define PRIxPTR /* see description */
#define PRIXPTR /* see description */
#define SCNdPTR /* see description */
#define SCNiPTR /* see description */
#define SCNoPTR /* see description */
#define SCNuPTR /* see description */
#define SCNxPTR /* see description */
```

---
*Source: https://en.cppreference.com/w/cpp/header/cinttypes*
