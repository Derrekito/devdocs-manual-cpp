# Standard library header <cuchar> (C++11)

This header was originally in the C standard library as `<uchar.h>`.

This header is part of the null-terminated multibyte strings library.

**Macros**

- **__STDC_UTF_16__ (C++11)** — indicates that UTF-16 encoding is used by
  mbrtoc16 and c16rtomb (macro constant)
- **__STDC_UTF_32__ (C++11)** — indicates that UTF-32 encoding is used by
  mbrtoc32 and c32rtomb (macro constant)

**Types**

- **mbstate_t** — conversion state information necessary to iterate multibyte
  character strings (class)
- **size_t** — unsigned integer type returned by the `sizeof` operator (typedef)

**Functions**

- **mbrtoc16 (C++11)** — converts a narrow multibyte character to UTF-16
  encoding (function)
- **c16rtomb (C++11)** — convert a 16-bit wide character to narrow multibyte
  string (function)
- **mbrtoc32 (C++11)** — converts a narrow multibyte character to UTF-32
  encoding (function)
- **c32rtomb (C++11)** — convert a 32-bit wide character to narrow multibyte
  string (function)
- **mbrtoc8 (C++20)** — converts a narrow multibyte character to UTF-8 encoding
  (function)
- **c8rtomb (C++20)** — converts UTF-8 string to narrow multibyte encoding
  (function)

### Synopsis

```cpp
namespace std {
  using mbstate_t = /* see description */;
  using size_t = /* see description */;

  size_t mbrtoc8(char8_t* pc8, const char* s, size_t n, mbstate_t* ps);
  size_t c8rtomb(char* s, char8_t c8, mbstate_t* ps);
  size_t mbrtoc16(char16_t* pc16, const char* s, size_t n, mbstate_t* ps);
  size_t c16rtomb(char* s, char16_t c16, mbstate_t* ps);
  size_t mbrtoc32(char32_t* pc32, const char* s, size_t n, mbstate_t* ps);
  size_t c32rtomb(char* s, char32_t c32, mbstate_t* ps);
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/cuchar*
