# std::wctob

```cpp
int wctob( std::wint_t c );
```

Narrows a wide character `c` if its multibyte character equivalent in the
initial shift state is a single byte.

This is typically possible for the characters from the ASCII character set,
since most multibyte encodings (such as UTF-8) use single bytes to encode those
characters.

### Parameters

- **c** — wide character to narrow

### Return value

`EOF` if `c` does not represent a multibyte character with length `1` in initial
shift state.

Otherwise, the single-byte representation of `c` as `unsigned char` converted to
`int`.

### Example

```cpp
#include <clocale>
#include <cwchar>
#include <iostream>

void try_narrowing(wchar_t c)
{
    int cn = std::wctob(c);
    if (cn != EOF)
        std::cout << '\'' << int(c) << "' narrowed to " << +cn << '\n';
    else
        std::cout << '\'' << int(c) << "' could not be narrowed\n";
}

int main()
{
    std::setlocale(LC_ALL, "th_TH.utf8");
    std::cout << std::hex << std::showbase << "In Thai UTF-8 locale:\n";
    try_narrowing(L'a');
    try_narrowing(L'๛');

    std::setlocale(LC_ALL, "th_TH.tis620");
    std::cout << "In Thai TIS-620 locale:\n";
    try_narrowing(L'a');
    try_narrowing(L'๛');
}
```

Output:

```text
In Thai UTF-8 locale:
'0x61' narrowed to 0x61
'0xe5b' could not be narrowed
In Thai TIS-620 locale:
'0x61' narrowed to 0x61
'0xe5b' narrowed to 0xfb
```

### See also

- **btowc** — widens a single-byte narrow character to wide character, if
  possible (function)
- **narrow** — narrows characters (public member function of
  `std::basic_ios<CharT,Traits>`)
- **narrow** — invokes `do_narrow` (public member function of
  `std::ctype<CharT>`)

**C documentation for `wctob`**

---
*Source: https://en.cppreference.com/w/cpp/string/multibyte/wctob*
