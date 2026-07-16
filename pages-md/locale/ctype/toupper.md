# std::ctype<CharT>::toupper, std::ctype<CharT>::do_toupper

```cpp
public:
CharT toupper( CharT c ) const;  // (1)
public:
const CharT* toupper( CharT* beg, const CharT* end ) const;  // (2)
protected:
virtual CharT do_toupper( CharT c ) const;  // (3)
protected:
virtual const CharT* do_toupper( CharT* beg, const CharT* end ) const;  // (4)
```

1,2) Public member function, calls the protected virtual member function
   `do_toupper` of the most derived class.

3) Converts the character `c` to upper case if an upper case form is defined by
   this locale.

4) For every character in the character array `[``beg``,``end``)`, for which an
   upper case form exists, replaces the character with that upper case form.

### Parameters

- **c** — character to convert
- **beg** — pointer to the first character in an array of characters to convert
- **end** — one past the end pointer for the array of characters to convert

### Return value

1,3) Upper case character or `c` if no upper case form is listed by this locale.

2,4) `end`

### Notes

Only 1:1 character mapping can be performed by this function, e.g. the uppercase
form of 'ß' is the two-character string "SS" (with some exceptions - see
«Capital ẞ»), which cannot be obtained by `do_toupper`.

### Example

```cpp
#include <iostream>
#include <locale>

void try_upper(const std::ctype<wchar_t>& f, wchar_t c)
{
    wchar_t up = f.toupper(c);
    if (up != c)
        std::wcout << "Upper case form of \'" << c << "' is " << up << '\n';
    else
        std::wcout << '\'' << c << "' has no upper case form\n";
}

int main()
{
    std::locale::global(std::locale("en_US.utf8"));
    std::wcout.imbue(std::locale());
    std::wcout << "In US English UTF-8 locale:\n";
    auto& f = std::use_facet<std::ctype<wchar_t>>(std::locale());
    try_upper(f, L's');
    try_upper(f, L'ſ');
    try_upper(f, L'δ');
    try_upper(f, L'ö');
    try_upper(f, L'ß');

    std::wstring str = L"Hello, World!";
    std::wcout << "Uppercase form of the string '" << str << "' is ";
    f.toupper(&str[0], &str[0] + str.size());
    std::wcout << '\'' << str << "'\n";
}
```

Output:

```text
In US English UTF-8 locale:
Upper case form of 's' is S
Upper case form of 'ſ' is S
Upper case form of 'δ' is Δ
Upper case form of 'ö' is Ö
'ß' has no upper case form
Uppercase form of the string 'Hello, World!' is 'HELLO, WORLD!'
```

### See also

- **tolower** — invokes `do_tolower` (public member function)
- **toupper** — converts a character to uppercase (function)
- **towupper** — converts a wide character to uppercase (function)

---
*Source: https://en.cppreference.com/w/cpp/locale/ctype/toupper*
