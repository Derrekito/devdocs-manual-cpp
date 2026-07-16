# std::ctype<CharT>::narrow, do_narrow

```cpp
public:
char narrow( CharT c, char dflt ) const;  // (1)
public:
const CharT* narrow( const CharT* beg, const CharT* end,
                     char dflt, char* dst ) const;  // (2)
protected:
virtual char do_narrow( CharT c, char dflt ) const;  // (3)
protected:
virtual const CharT* do_narrow( const CharT* beg, const CharT* end,
                                char dflt, char* dst ) const;  // (4)
```

1,2) Public member function, calls the corresponding protected virtual member
   function `do_narrow` overload of the most derived class. Overload (1) calls
   `do_narrow(c, dflt)`, overload (2) calls `do_narrow(beg, end, dflt, dst)`.

3) Converts the (possibly wide) character `c` to multibyte representation if the
   character can be represented with a single byte (for example, ASCII
   characters in UTF-8 encoding are single bytes). Returns `dflt` if such
   conversion does not exist.

4) For every character in the character array `[``beg``,``end``)`, writes
   narrowed characters (or `dflt` whenever narrowing fails) to the successive
   locations in the character array pointed to by `dst`.

Narrowing is always successful and is always reversible (by calling `widen()`)
for all characters from the basic source character set(until C++23)basic
character set(since C++23).

- i.e. `do_widen(do_narrow(c, 0)) == c` always holds for any character `c` in
  the basic source character set(until C++23)basic character set(since C++23).

Narrowing, if successful, preserves all character classification categories
known to `is()`.

- i.e. `is(m, c) || !ctc.is(m, do_narrow(c, dflt))` is always `true` for any
  named `ctype` category with a `ctype<char>` facet `ctc` and `ctype_base::mask`
  value `m` (unless `do_narrow` returns `dflt`).

Narrowing of any digit character guarantees that if the result is subtracted
from the character literal `'0'`, the difference equals the digit value of the
original character.

- i.e. for any digit character `c`, the expression `(do_narrow(c, dflt) - ’0’)`
  evaluates to the digit value of the character.

### Parameters

- **c** — character to convert
- **dflt** — default value to produce if the conversion fails
- **beg** — pointer to the first character in an array of characters to convert
- **end** — one past the end pointer for the array of characters to convert
- **dst** — pointer to the first element of the array of characters to fill

### Return value

1,3) Narrowed character or `dflt` if narrowing fails.

2,4) `end`

### Example

```cpp
#include <iostream>
#include <locale>

void try_narrow(const std::ctype<wchar_t>& f, wchar_t c)
{
    char n = f.narrow(c, 0);
    if (n)
        std::wcout << '\'' << c << "' narrowed to " << +(unsigned char)n << '\n';
    else
        std::wcout << '\'' << c << "' could not be narrowed\n";
}

int main()
{
    std::locale::global(std::locale("en_US.utf8"));
    std::wcout.imbue(std::locale());
    std::wcout << std::hex << std::showbase << "In US English UTF-8 locale:\n";
    auto& f = std::use_facet<std::ctype<wchar_t>>(std::locale());
    try_narrow(f, L'A');
    try_narrow(f, L'Ａ');
    try_narrow(f, L'ě');

    std::locale::global(std::locale("cs_CZ.iso88592"));
    auto& f2 = std::use_facet<std::ctype<wchar_t>>(std::locale());
    std::wcout << "In Czech ISO-8859-2 locale:\n";
    try_narrow(f2, L'A');
    try_narrow(f2, L'Ａ');
    try_narrow(f2, L'ě');
}
```

Possible output:

```text
In US English UTF-8 locale:
'A' narrowed to 0x41
'Ａ' could not be narrowed
'ě' could not be narrowed
In Czech ISO-8859-2 locale:
'A' narrowed to 0x41
'Ａ' could not be narrowed
'ě' narrowed to 0xec
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 126 | C++98 | 1. the code representing reversibility was
      `do_widen(do_narrow(c), 0) == c` 2. the code representing category
      preservation was `is(m, c) || !ctc.is(m, do_narrow(c), dflt)` | corrected
      both
  LWG 153 | C++98 | `narrow` always called overload (4) | calls the
      corresponding overload

### See also

- **widen** — invokes `do_widen` (public member function)
- **narrow** — narrows characters (public member function of
  `std::basic_ios<CharT,Traits>`)
- **wctob** — narrows a wide character to a single-byte narrow character, if
  possible (function)

---
*Source: https://en.cppreference.com/w/cpp/locale/ctype/narrow*
