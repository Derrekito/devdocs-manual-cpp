# std::iscntrl

```cpp
int iscntrl( int ch );
```

Checks if the given character is a control character as classified by the
currently installed C locale. In the default, `"C"` locale, the control
characters are the characters with the codes `0x00-0x1F` and `0x7F`.

The behavior is undefined if the value of `ch` is not representable as `unsigned
char` and is not equal to `EOF`.

### Parameters

- **ch** — character to classify

### Return value

Non-zero value if the character is a control character, zero otherwise.

### Notes

Like all other functions from `<cctype>`, the behavior of `std::iscntrl` is
undefined if the argument's value is neither representable as `unsigned char`
nor equal to `EOF`. To use these functions safely with plain `char`s (or `signed
char`s), the argument should first be converted to `unsigned char`:

```cpp
bool my_iscntrl(char ch)
{
    return std::iscntrl(static_cast<unsigned char>(ch));
}
```

Similarly, they should not be directly used with standard algorithms when the
iterator's value type is `char` or `signed char`. Instead, convert the value to
`unsigned char` first:

```cpp
int count_cntrls(const std::string& s)
{
    return std::count_if(s.begin(), s.end(),
                      // static_cast<int(*)(int)>(std::iscntrl)         // wrong
                      // [](int c){ return std::iscntrl(c); }           // wrong
                      // [](char c){ return std::iscntrl(c); }          // wrong
                         [](unsigned char c){ return std::iscntrl(c); } // correct
                        );
}
```

### Example

```cpp
#include <cctype>
#include <clocale>
#include <iostream>

int main()
{
    unsigned char c = '\x94'; // the control code CCH in ISO-8859-1

    std::cout << "iscntrl(\'\\x94\', default C locale) returned "
              << std::boolalpha << !!std::iscntrl(c) << '\n';

    std::setlocale(LC_ALL, "en_GB.iso88591");
    std::cout << "iscntrl(\'\\x94\', ISO-8859-1 locale) returned "
              << !!std::iscntrl(c) << '\n';

}
```

Possible output:

```text
iscntrl('\x94', default C locale) returned false
iscntrl('\x94', ISO-8859-1 locale) returned true
```

### See also

- **iscntrl(std::locale)** — checks if a character is classified as a control
  character by a locale (function template)
- **iswcntrl** — checks if a wide character is a control character (function)

**C documentation for `iscntrl`**

  ASCII values | characters | **`iscntrl`** `iswcntrl` | `isprint` `iswprint` |
      `isspace` `iswspace` | `isblank` `iswblank` | `isgraph` `iswgraph` |
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
*Source: https://en.cppreference.com/w/cpp/string/byte/iscntrl*
