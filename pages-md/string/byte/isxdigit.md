# std::isxdigit

```cpp
int isxdigit( int ch );
```

Checks if the given character is a hexadecimal numeric character
(0123456789abcdefABCDEF).

The behavior is undefined if the value of `ch` is not representable as `unsigned
char` and is not equal to `EOF`.

### Parameters

- **ch** — character to classify

### Return value

Non-zero value if the character is a hexadecimal numeric character, zero
otherwise.

### Notes

`std::isdigit` and `std::isxdigit` are the only standard narrow character
classification functions that are not affected by the currently installed C
locale. although some implementations (e.g. Microsoft in 1252 codepage) may
classify additional single-byte characters as digits.

Like all other functions from `<cctype>`, the behavior of `std::isxdigit` is
undefined if the argument's value is neither representable as `unsigned char`
nor equal to `EOF`. To use these functions safely with plain `char`s (or `signed
char`s), the argument should first be converted to `unsigned char`:

```cpp
bool my_isxdigit(char ch)
{
    return std::isxdigit(static_cast<unsigned char>(ch));
}
```

Similarly, they should not be directly used with standard algorithms when the
iterator's value type is `char` or `signed char`. Instead, convert the value to
`unsigned char` first:

```cpp
int count_xdigits(const std::string& s)
{
    return std::count_if(s.begin(), s.end(),
                      // static_cast<int(*)(int)>(std::isxdigit)         // wrong
                      // [](int c){ return std::isxdigit(c); }           // wrong
                      // [](char c){ return std::isxdigit(c); }          // wrong
                         [](unsigned char c){ return std::isxdigit(c); } // correct
                        );
}
```

### Example

```cpp
#include <cctype>
#include <climits>
#include <iostream>

int main()
{
    for (int c = 0; UCHAR_MAX >= c; ++c)
        if (isxdigit(c))
            std::cout << static_cast<char>(c);
    std::cout << '\n';
}
```

Output:

```text
0123456789ABCDEFabcdef
```

### See also

- **isxdigit(std::locale)** — checks if a character is classified as a
  hexadecimal digit by a locale (function template)
- **iswxdigit** — checks if a wide character is a hexadecimal character
  (function)

**C documentation for `isxdigit`**

  ASCII values | characters | `iscntrl` `iswcntrl` | `isprint` `iswprint` |
      `isspace` `iswspace` | `isblank` `iswblank` | `isgraph` `iswgraph` |
      `ispunct` `iswpunct` | `isalnum` `iswalnum` | `isalpha` `iswalpha` |
      `isupper` `iswupper` | `islower` `iswlower` | `isdigit` `iswdigit` |
      **`isxdigit`** `iswxdigit`
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
*Source: https://en.cppreference.com/w/cpp/string/byte/isxdigit*
