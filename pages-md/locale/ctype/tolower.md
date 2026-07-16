# std::ctype<CharT>::tolower, std::ctype<CharT>::do_tolower

```cpp
public:
CharT tolower( CharT c ) const;  // (1)
public:
const CharT* tolower( CharT* beg, const CharT* end ) const;  // (2)
protected:
virtual CharT do_tolower( CharT c ) const;  // (3)
protected:
virtual const CharT* do_tolower( CharT* beg, const CharT* end ) const;  // (4)
```

1,2) Public member function, calls the protected virtual member function
   `do_tolower` of the most derived class.

3) Converts the character `c` to lower case if a lower case form is defined by
   this locale.

4) For every character in the character array `[``beg``,``end``)`, for which a
   lower case form exists, replaces the character with that lower case form.

### Parameters

- **c** — character to convert
- **beg** — pointer to the first character in an array of characters to convert
- **end** — one past the end pointer for the array of characters to convert

### Return value

1,3) Lower case character or `c` if no lower case form is listed by this locale.

2,4) `end`

### Notes

Only 1:1 character mapping can be performed by this function, e.g. the Greek
uppercase letter 'Σ' has two lowercase forms, depending on the position in a
word: 'σ' and 'ς'. A call to `do_tolower` cannot be used to obtain the correct
lowercase form in this case.

### Example

```cpp
#include <iostream>
#include <locale>

void try_lower(const std::ctype<wchar_t>& f, wchar_t c)
{
    wchar_t up = f.tolower(c);
    if (up != c)
        std::wcout << "Lower case form of \'" << c << "' is " << up << '\n';
    else
        std::wcout << '\'' << c << "' has no lower case form\n";
}

int main()
{
    std::locale::global(std::locale("en_US.utf8"));
    std::wcout.imbue(std::locale());
    std::wcout << "In US English UTF-8 locale:\n";
    auto& f = std::use_facet<std::ctype<wchar_t>>(std::locale());
    try_lower(f, L'Σ');
    try_lower(f, L'Ɛ');
    try_lower(f, L'Ａ');

    std::wstring str = L"HELLo, wORLD!";
    std::wcout << "Lowercase form of the string '" << str << "' is ";
    f.tolower(&str[0], &str[0] + str.size());
    std::wcout << '\'' << str << "'\n";
}
```

Output:

```text
In US English UTF-8 locale:
Lower case form of 'Σ' is σ
Lower case form of 'Ɛ' is ɛ
Lower case form of 'Ａ' is ａ
Lowercase form of the string 'HELLo, wORLD!' is 'hello, world!'
```

### See also

- **toupper** — invokes `do_toupper` (public member function)
- **tolower** — converts a character to lowercase (function)
- **towlower** — converts a wide character to lowercase (function)

---
*Source: https://en.cppreference.com/w/cpp/locale/ctype/tolower*
