# std::codecvt_utf8_utf16

```cpp
template<
    class Elem,
    unsigned long Maxcode = 0x10ffff,
    std::codecvt_mode Mode = (std::codecvt_mode)0 >
class codecvt_utf8_utf16
    : public std::codecvt<Elem, char, std::mbstate_t>;  // (since C++11) (deprecated in C++17) (removed in C++26)
```

`std::codecvt_utf8_utf16` is a `std::codecvt` facet which encapsulates
conversion between a UTF-8 encoded byte string and UTF-16 encoded character
string. If `Elem` is a 32-bit type, one UTF-16 code unit will be stored in each
32-bit character of the output sequence.

This is an N:M conversion facet, and cannot be used with `std::basic_filebuf`
(which only permits 1:N conversions, such as UTF-32/UTF-8, between the internal
and the external encodings). This facet can be used with `std::wstring_convert`.

### Template Parameters

- **Elem** — either char16_t, char32_t, or wchar_t
- **Maxcode** — the largest value of `Elem` that this facet will read or write
  without error
- **Mode** — a constant of type `std::codecvt_mode`

### Member functions

- ****(constructor)**** — constructs a new `codecvt_utf8_utf16` facet (public
  member function)
- ****(destructor)**** — destroys a `codecvt_utf8_utf16` facet (public member
  function)

## std::codecvt_utf8_utf16::codecvt_utf8_utf16

```cpp
explicit codecvt_utf8_utf16( std::size_t refs = 0 );
```

Constructs a new `std::codecvt_utf8_utf16` facet, passes the initial reference
counter `refs` to the base class.

### Parameters

- **refs** — the number of references that link to the facet

## std::codecvt_utf8_utf16::~codecvt_utf8_utf16

```cpp
~codecvt_utf8_utf16();
```

Destroys the facet. Unlike the locale-managed facets, this facet's destructor is
public.

## Inherited from std::codecvt

### Member types

- **`intern_type`** — `internT`
- **`extern_type`** — `externT`
- **`state_type`** — `stateT`

### Member objects

- **`id` (static)** — `std::locale::id`

### Member functions

- **out** — invokes `do_out` (public member function of
  `std::codecvt<InternT,ExternT,StateT>`)
- **in** — invokes `do_in` (public member function of
  `std::codecvt<InternT,ExternT,StateT>`)
- **unshift** — invokes `do_unshift` (public member function of
  `std::codecvt<InternT,ExternT,StateT>`)
- **encoding** — invokes `do_encoding` (public member function of
  `std::codecvt<InternT,ExternT,StateT>`)
- **always_noconv** — invokes `do_always_noconv` (public member function of
  `std::codecvt<InternT,ExternT,StateT>`)
- **length** — invokes `do_length` (public member function of
  `std::codecvt<InternT,ExternT,StateT>`)
- **max_length** — invokes `do_max_length` (public member function of
  `std::codecvt<InternT,ExternT,StateT>`)

### Protected member functions

- **do_out [virtual]** — converts a string from `InternT` to `ExternT`, such as
  when writing to file (virtual protected member function of
  `std::codecvt<InternT,ExternT,StateT>`)
- **do_in [virtual]** — converts a string from `ExternT` to `InternT`, such as
  when reading from file (virtual protected member function of
  `std::codecvt<InternT,ExternT,StateT>`)
- **do_unshift [virtual]** — generates the termination character sequence of
  `ExternT` characters for incomplete conversion (virtual protected member
  function of `std::codecvt<InternT,ExternT,StateT>`)
- **do_encoding [virtual]** — returns the number of `ExternT` characters
  necessary to produce one `InternT` character, if constant (virtual protected
  member function of `std::codecvt<InternT,ExternT,StateT>`)
- **do_always_noconv [virtual]** — tests if the facet encodes an identity
  conversion for all valid argument values (virtual protected member function of
  `std::codecvt<InternT,ExternT,StateT>`)
- **do_length [virtual]** — calculates the length of the `ExternT` string that
  would be consumed by conversion into given `InternT` buffer (virtual protected
  member function of `std::codecvt<InternT,ExternT,StateT>`)
- **do_max_length [virtual]** — returns the maximum number of `ExternT`
  characters that could be converted into a single `InternT` character (virtual
  protected member function of `std::codecvt<InternT,ExternT,StateT>`)

## Inherited from std::codecvt_base

- **enum result { ok, partial, error, noconv };** — Unscoped enumeration type

- **`ok`** — conversion was completed with no error
- **`partial`** — not all source characters were converted
- **`error`** — encountered an invalid character
- **`noconv`** — no conversion required, input and output types are the same

### Example

```cpp
#include <cassert>
#include <codecvt>
#include <cstdint>
#include <iostream>
#include <locale>
#include <string>

int main()
{
    std::string u8 = "z\u00df\u6c34\U0001f34c";
    std::u16string u16 = u"z\u00df\u6c34\U0001f34c";

    // UTF-8 to UTF-16/char16_t
    std::u16string u16_conv = std::wstring_convert<
        std::codecvt_utf8_utf16<char16_t>, char16_t>{}.from_bytes(u8);
    assert(u16 == u16_conv);
    std::cout << "UTF-8 to UTF-16 conversion produced " << u16_conv.size()
              << " code units:\n" << std::showbase << std::hex;
    for (char16_t c : u16_conv)
        std::cout << static_cast<std::uint16_t>(c) << ' ';

    // UTF-16/char16_t to UTF-8
    std::string u8_conv = std::wstring_convert<
        std::codecvt_utf8_utf16<char16_t>, char16_t>{}.to_bytes(u16);
    assert(u8 == u8_conv);
    std::cout << "\nUTF-16 to UTF-8 conversion produced "
              << std::dec << u8_conv.size() << " bytes:\n" << std::hex;
    for (char c : u8_conv)
        std::cout << +static_cast<unsigned char>(c) << ' ';
    std::cout << '\n';
}
```

Output:

```text
UTF-8 to UTF-16 conversion produced 5 code units:
0x7a 0xdf 0x6c34 0xd83c 0xdf4c
UTF-16 to UTF-8 conversion produced 10 bytes:
0x7a 0xc3 0x9f 0xe6 0xb0 0xb4 0xf0 0x9f 0x8d 0x8c
```

### See also

  Character conversions | locale-defined multibyte (UTF-8, GB18030) | UTF-8 |
      UTF-16
  UTF-16 | `mbrtoc16` / `c16rtomb` (with C11's DR488) |
      `codecvt`<char16_t,char,mbstate_t> **`codecvt_utf8_utf16`**<char16_t>
      **`codecvt_utf8_utf16`**<char32_t> **`codecvt_utf8_utf16`**<wchar_t> | N/A
  UCS-2 | `c16rtomb` (without C11's DR488) | `codecvt_utf8`<char16_t> |
      `codecvt_utf16`<char16_t>
  UTF-32 | `mbrtoc32` / `c32rtomb` | `codecvt`<char32_t,char,mbstate_t>
      `codecvt_utf8`<char32_t> | `codecvt_utf16`<char32_t>
  system wchar_t: UTF-32 (non-Windows) UCS-2 (Windows) | `mbsrtowcs` /
      `wcsrtombs` `use_facet`<`codecvt` <wchar_t,char,mbstate_t>>(`locale`) |
      `codecvt_utf8`<wchar_t> | `codecvt_utf16`<wchar_t>

- **codecvt** — converts between character encodings, including UTF-8, UTF-16,
  UTF-32 (class template)
- **codecvt_mode (C++11)(deprecated in C++17)(removed in C++26)** — tags to
  alter behavior of the standard codecvt facets (enum)
- **codecvt_utf8 (C++11)(deprecated in C++17)(removed in C++26)** — converts
  between UTF-8 and UCS-2/UCS-4 (class template)
- **codecvt_utf16 (C++11)(deprecated in C++17)(removed in C++26)** — converts
  between UTF-16 and UCS-2/UCS-4 (class template)

---
*Source: https://en.cppreference.com/w/cpp/locale/codecvt_utf8_utf16*
