# Standard library header <cwctype>

This header was originally in the C standard library as `<wctype.h>`.

This header is part of the C-style null-terminated wide strings library.

### Types

- **`wctrans_t`** — scalar type that holds locale-specific character mapping
- **`wctype_t`** — scalar type that holds locale-specific character
  classification
- **`wint_t`** — integer type that can hold any valid wide character and at
  least one more value

### Macros

- **WEOF** — a non-character value of type wint_t used to indicate errors (macro
  constant)

### Functions

**Character classification**

- **iswalnum** — checks if a wide character is alphanumeric (function)
- **iswalpha** — checks if a wide character is alphabetic (function)
- **iswlower** — checks if a wide character is lowercase (function)
- **iswupper** — checks if a wide character is an uppercase character (function)
- **iswdigit** — checks if a wide character is a digit (function)
- **iswxdigit** — checks if a wide character is a hexadecimal character
  (function)
- **iswcntrl** — checks if a wide character is a control character (function)
- **iswgraph** — checks if a wide character is a graphical character (function)
- **iswspace** — checks if a wide character is a space character (function)
- **iswblank (C++11)** — checks if a wide character is a blank character
  (function)
- **iswprint** — checks if a wide character is a printing character (function)
- **iswpunct** — checks if a wide character is a punctuation character
  (function)
- **iswctype** — classifies a wide character according to the specified
  `LC_CTYPE` category (function)
- **wctype** — looks up a character classification category in the current C
  locale (function)

**Character manipulation**

- **towlower** — converts a wide character to lowercase (function)
- **towupper** — converts a wide character to uppercase (function)
- **towctrans** — performs character mapping according to the specified
  `LC_CTYPE` mapping category (function)
- **wctrans** — looks up a character mapping category in the current C locale
  (function)

---
*Source: https://en.cppreference.com/w/cpp/header/cwctype*
