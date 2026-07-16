# Standard library header <cctype>

This header was originally in the C standard library as `<ctype.h>`.

This header is part of the null-terminated byte strings library.

**Functions**

- **isalnum** — checks if a character is alphanumeric (function)
- **isalpha** — checks if a character is alphabetic (function)
- **islower** — checks if a character is lowercase (function)
- **isupper** — checks if a character is an uppercase character (function)
- **isdigit** — checks if a character is a digit (function)
- **isxdigit** — checks if a character is a hexadecimal character (function)
- **iscntrl** — checks if a character is a control character (function)
- **isgraph** — checks if a character is a graphical character (function)
- **isspace** — checks if a character is a space character (function)
- **isblank (C++11)** — checks if a character is a blank character (function)
- **isprint** — checks if a character is a printing character (function)
- **ispunct** — checks if a character is a punctuation character (function)
- **tolower** — converts a character to lowercase (function)
- **toupper** — converts a character to uppercase (function)

### Synopsis

```cpp
namespace std {
  int isalnum(int c);
  int isalpha(int c);
  int isblank(int c);
  int iscntrl(int c);
  int isdigit(int c);
  int isgraph(int c);
  int islower(int c);
  int isprint(int c);
  int ispunct(int c);
  int isspace(int c);
  int isupper(int c);
  int isxdigit(int c);
  int tolower(int c);
  int toupper(int c);
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/cctype*
