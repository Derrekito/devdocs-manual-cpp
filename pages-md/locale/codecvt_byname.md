# std::codecvt_byname

```cpp
template< class InternT, class ExternT, class State >
class codecvt_byname : public std::codecvt<InternT, ExternT, State>;
```

`std::codecvt_byname` is a `std::codecvt` facet which encapsulates
multibyte/wide character conversion rules of a locale specified at its
construction.

### Specializations

The standard library is guaranteed to provide the following specializations:

- **std::codecvt_byname<char, char, std::mbstate_t>** — identity conversion
- **std::codecvt_byname<char16_t, char, std::mbstate_t>** — conversion between
  UTF-16 and UTF-8 (since C++11)(deprecated in C++20)
- **std::codecvt_byname<char16_t, char8_t, std::mbstate_t>** — conversion
  between UTF-16 and UTF-8 (since C++20)
- **std::codecvt_byname<char32_t, char, std::mbstate_t>** — conversion between
  UTF-32 and UTF-8 (since C++11)(deprecated in C++20)
- **std::codecvt_byname<char32_t, char8_t, std::mbstate_t>** — conversion
  between UTF-32 and UTF-8 (since C++20)
- **std::codecvt_byname<wchar_t, char, std::mbstate_t>** — locale-specific
  conversion between wide string and narrow character sets

### Member functions

- ****(constructor)**** — constructs a new `codecvt_byname` facet (public member
  function)
- ****(destructor)**** — destroys a `codecvt_byname` facet (protected member
  function)

## std::codecvt_byname::codecvt_byname

```cpp
explicit codecvt_byname( const char* name, std::size_t refs = 0 );
explicit codecvt_byname( const std::string& name, std::size_t refs = 0 );  // (since C++11)
```

Constructs a new `std::codecvt_byname` facet for a locale with `name`.

`refs` is used for resource management: if `refs == 0`, the implementation
destroys the facet, when the last `std::locale` object holding it is destroyed.
Otherwise, the object is not destroyed.

### Parameters

- **name** — the name of the locale
- **refs** — the number of references that link to the facet

## std::codecvt_byname::~codecvt_byname

```cpp
protected:
~codecvt_byname();
```

Destroys the facet.

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

This example demonstrates reading a GB18030-encoded file using the codecvt facet
from a GB18030-aware locale.

```cpp
#include <fstream>
#include <iostream>
#include <locale>
#include <string>

int main()
{
    // GB18030 narrow multibyte encoding
    std::ofstream("text.txt") << "\x7a"              // letter 'z', U+007a
                                 "\x81\x30\x89\x38"  // letter 'ß', U+00df
                                 "\xcb\xae"          // CJK ideogram '水' (water), U+6c34
                                 "\x94\x32\xbc\x35"; // musical sign '𝄋' (segno), U+1d10b

    std::wifstream fin("text.txt");
    fin.imbue(std::locale(fin.getloc(),
              new std::codecvt_byname<wchar_t, char, std::mbstate_t>("zh_CN.gb18030")));

    for (wchar_t c; fin.get(c);)
        std::cout << std::hex << std::showbase << static_cast<unsigned>(c) << '\n';
}
```

Possible output:

```text
0x7a
0xdf
0x6c34
0x1d10b
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 21 | C++98 | the standard library did not need to provide any
      `std::codecvt_byname` specializations | two specializations are required

### See also

- **codecvt** — converts between character encodings, including UTF-8, UTF-16,
  UTF-32 (class template)

---
*Source: https://en.cppreference.com/w/cpp/locale/codecvt_byname*
