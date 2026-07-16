# std::iswblank

```cpp
int iswblank( std::wint_t ch );  // (since C++11)
```

Checks if the given wide character is classified as blank character (that is, a
whitespace character used to separate words in a sentence) by the current C
locale. In the default C locale, only space (`0x20`) and horizontal tab (`0x09`)
are blank characters.

If the value of `ch` is neither representable as a `wchar_t` nor equal to the
value of the macro `WEOF`, the behavior is undefined.

### Parameters

- **ch** — wide character

### Return value

Non-zero value if the wide character is a blank character, zero otherwise.

### Notes

ISO 30112 defines POSIX blank characters as Unicode characters U+0009, U+0020,
U+1680, U+180E, U+2000..U+2006, U+2008, U+200A, U+205F, and U+3000.

### Example

```cpp
#include <clocale>
#include <cwctype>
#include <iostream>

int main()
{
    wchar_t c = L'\u3000'; // Ideographic space ('　')

    std::cout << std::hex << std::showbase << std::boolalpha;
    std::cout << "in the default locale, iswblank(" << (std::wint_t)c << ") = "
              << (bool)std::iswblank(c) << '\n';
    std::setlocale(LC_ALL, "en_US.utf8");
    std::cout << "in Unicode locale, iswblank(" << (std::wint_t)c << ") = "
              << (bool)std::iswblank(c) << '\n';
}
```

Output:

```text
in the default locale, iswblank(0x3000) = false
in Unicode locale, iswblank(0x3000) = true
```

### See also

- **isblank(std::locale) (C++11)** — checks if a character is classified as a
  blank character by a locale (function template)
- **isblank (C++11)** — checks if a character is a blank character (function)

**C documentation for `iswblank`**

  ASCII values | characters | `iscntrl` `iswcntrl` | `isprint` `iswprint` |
      `isspace` `iswspace` | `isblank` **`iswblank`** | `isgraph` `iswgraph` |
      `ispunct` `iswpunct` | `isalnum` `iswalnum` | `isalpha` `iswalpha` |
      `isupper` `iswupper` | `islower` `iswlower` | `isdigit` `iswdigit` |
      `isxdigit` `iswxdigit`
  decimal | hexadecimal | octal
  0–8 | `\x0`–`\x8` | `\0`–`\10` | control codes (`NUL`, etc.) | **`≠0`** |
      **`0`** | **`0`** | **`0`** | **`0`** | **`0`** | **`0`** | **`0`** |
      **`0`** | **`0`** | **`0`** | **`0`**
  9 | `\x9` | `\11` | tab (`\t`) | **`≠0`** | **`0`** | **`≠0`** | **`≠0`** |
      **`0`** | **`0`** | **`0`** | **`0`** | **`0`** | **`0`** | **`0`** |
      **`0`**
  10–13 | `\xA`–`\xD` | `\12`–`\15` | whitespaces (`\n`, `\v`, `\f`, `\r`) |
      **`≠0`** | **`0`** | **`≠0`** | **`0`** | **`0`** | **`0`** | **`0`** |
      **`0`** | **`0`** | **`0`** | **`0`** | **`0`**
  14–31 | `\xE`–`\x1F` | `\16`–`\37` | control codes | **`≠0`** | **`0`** |
      **`0`** | **`0`** | **`0`** | **`0`** | **`0`** | **`0`** | **`0`** |
      **`0`** | **`0`** | **`0`**
  32 | `\x20` | `\40` | space | **`0`** | **`≠0`** | **`≠0`** | **`≠0`** |
      **`0`** | **`0`** | **`0`** | **`0`** | **`0`** | **`0`** | **`0`** |
      **`0`**
  33–47 | `\x21`–`\x2F` | `\41`–`\57` | `!"#$%&'()*+,-./` | **`0`** | **`≠0`** |
      **`0`** | **`0`** | **`≠0`** | **`≠0`** | **`0`** | **`0`** | **`0`** |
      **`0`** | **`0`** | **`0`**
  48–57 | `\x30`–`\x39` | `\60`–`\71` | `0123456789` | **`0`** | **`≠0`** |
      **`0`** | **`0`** | **`≠0`** | **`0`** | **`≠0`** | **`0`** | **`0`** |
      **`0`** | **`≠0`** | **`≠0`**
  58–64 | `\x3A`–`\x40` | `\72`–`\100` | `:;<=>?@` | **`0`** | **`≠0`** |
      **`0`** | **`0`** | **`≠0`** | **`≠0`** | **`0`** | **`0`** | **`0`** |
      **`0`** | **`0`** | **`0`**
  65–70 | `\x41`–`\x46` | `\101`–`\106` | `ABCDEF` | **`0`** | **`≠0`** |
      **`0`** | **`0`** | **`≠0`** | **`0`** | **`≠0`** | **`≠0`** | **`≠0`** |
      **`0`** | **`0`** | **`≠0`**
  71–90 | `\x47`–`\x5A` | `\107`–`\132` | `GHIJKLMNOP` `QRSTUVWXYZ` | **`0`** |
      **`≠0`** | **`0`** | **`0`** | **`≠0`** | **`0`** | **`≠0`** | **`≠0`** |
      **`≠0`** | **`0`** | **`0`** | **`0`**
  91–96 | `\x5B`–`\x60` | `\133`–`\140` | `[\]^_`` | **`0`** | **`≠0`** |
      **`0`** | **`0`** | **`≠0`** | **`≠0`** | **`0`** | **`0`** | **`0`** |
      **`0`** | **`0`** | **`0`**
  97–102 | `\x61`–`\x66` | `\141`–`\146` | `abcdef` | **`0`** | **`≠0`** |
      **`0`** | **`0`** | **`≠0`** | **`0`** | **`≠0`** | **`≠0`** | **`0`** |
      **`≠0`** | **`0`** | **`≠0`**
  103–122 | `\x67`–`\x7A` | `\147`–`\172` | `ghijklmnop` `qrstuvwxyz` | **`0`**
      | **`≠0`** | **`0`** | **`0`** | **`≠0`** | **`0`** | **`≠0`** | **`≠0`**
      | **`0`** | **`≠0`** | **`0`** | **`0`**
  123–126 | `\x7B`–`\x7E` | `\172`–`\176` | `{|}~` | **`0`** | **`≠0`** |
      **`0`** | **`0`** | **`≠0`** | **`≠0`** | **`0`** | **`0`** | **`0`** |
      **`0`** | **`0`** | **`0`**
  127 | `\x7F` | `\177` | backspace character (`DEL`) | **`≠0`** | **`0`** |
      **`0`** | **`0`** | **`0`** | **`0`** | **`0`** | **`0`** | **`0`** |
      **`0`** | **`0`** | **`0`**

---
*Source: https://en.cppreference.com/w/cpp/string/wide/iswblank*
