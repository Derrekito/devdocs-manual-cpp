# std::tolower

```cpp
int tolower( int ch );
```

Converts the given character to lowercase according to the character conversion
rules defined by the currently installed C locale.

In the default `"C"` locale, the following uppercase letters
`ABCDEFGHIJKLMNOPQRSTUVWXYZ` are replaced with respective lowercase letters
`abcdefghijklmnopqrstuvwxyz`.

### Parameters

- **ch** — character to be converted. If the value of `ch` is not representable
  as `unsigned char` and does not equal `EOF`, the behavior is undefined

### Return value

Lowercase version of `ch` or unmodified `ch` if no lowercase version is listed
in the current C locale.

### Notes

Like all other functions from `<cctype>`, the behavior of `std::tolower` is
undefined if the argument's value is neither representable as `unsigned char`
nor equal to `EOF`. To use these functions safely with plain `char`s (or `signed
char`s), the argument should first be converted to `unsigned char`:

```cpp
char my_tolower(char ch)
{
    return static_cast<char>(std::tolower(static_cast<unsigned char>(ch)));
}
```

Similarly, they should not be directly used with standard algorithms when the
iterator's value type is `char` or `signed char`. Instead, convert the value to
`unsigned char` first:

```cpp
std::string str_tolower(std::string s)
{
    std::transform(s.begin(), s.end(), s.begin(),
                // static_cast<int(*)(int)>(std::tolower)         // wrong
                // [](int c){ return std::tolower(c); }           // wrong
                // [](char c){ return std::tolower(c); }          // wrong
                   [](unsigned char c){ return std::tolower(c); } // correct
                  );
    return s;
}
```

### Example

```cpp
#include <cctype>
#include <clocale>
#include <iostream>

int main()
{
    unsigned char c = '\xb4'; // the character Ž in ISO-8859-15
                              // but ´ (acute accent) in ISO-8859-1

    std::setlocale(LC_ALL, "en_US.iso88591");
    std::cout << std::hex << std::showbase;
    std::cout << "in iso8859-1, tolower('0xb4') gives "
              << std::tolower(c) << '\n';
    std::setlocale(LC_ALL, "en_US.iso885915");
    std::cout << "in iso8859-15, tolower('0xb4') gives "
              << std::tolower(c) << '\n';
}
```

Possible output:

```text
in iso8859-1, tolower('0xb4') gives 0xb4
in iso8859-15, tolower('0xb4') gives 0xb8
```

### See also

- **toupper** — converts a character to uppercase (function)
- **tolower(std::locale)** — converts a character to lowercase using the `ctype`
  facet of a locale (function template)
- **towlower** — converts a wide character to lowercase (function)

**C documentation for `tolower`**

---
*Source: https://en.cppreference.com/w/cpp/string/byte/tolower*
