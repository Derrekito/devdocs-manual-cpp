# Standard library header <climits>

This header was originally in the C standard library as `<limits.h>`.

This header is part of the type support library, in particular it's part of the
C numeric limits interface.

### Macros

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

### Synopsis

```cpp
#define CHAR_BIT      /* see definition */
#define SCHAR_MIN     /* see definition */
#define SCHAR_MAX     /* see definition */
#define UCHAR_MAX     /* see definition */
#define CHAR_MIN      /* see definition */
#define CHAR_MAX      /* see definition */
#define MB_LEN_MAX    /* see definition */
#define SHRT_MIN      /* see definition */
#define SHRT_MAX      /* see definition */
#define USHRT_MAX     /* see definition */
#define INT_MIN       /* see definition */
#define INT_MAX       /* see definition */
#define UINT_MAX      /* see definition */
#define LONG_MIN      /* see definition */
#define LONG_MAX      /* see definition */
#define ULONG_MAX     /* see definition */
#define LLONG_MIN     /* see definition */
#define LLONG_MAX     /* see definition */
#define ULLONG_MAX    /* see definition */
```

---
*Source: https://en.cppreference.com/w/cpp/header/climits*
