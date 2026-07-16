# std::ctype_byname

```cpp
template< class CharT >
class ctype_byname : public std::ctype<CharT>;
```

`std::ctype_byname` is a `std::ctype` facet which encapsulates character
classification rules of the locale specified at its construction.

### Specializations

The standard library is guaranteed to provide the following specializations:

- **std::ctype_byname<char>** — provides narrow character classification. This
  specialization uses table lookup for character classification
- **std::ctype_byname<wchar_t>** — provides wide character classification

### Member types

- **`mask`** — typename std::ctype<CharT>::mask

### Member functions

- ****(constructor)**** — constructs a new `ctype_byname` facet (public member
  function)
- ****(destructor)**** — destroys a `ctype_byname` facet (protected member
  function)

## std::ctype_byname::ctype_byname

```cpp
explicit ctype_byname( const char* name, std::size_t refs = 0 );
explicit ctype_byname( const std::string& name, std::size_t refs = 0 );  // (since C++11)
```

Constructs a new `std::ctype_byname` facet for a locale with `name`.

`refs` is used for resource management: if `refs == 0`, the implementation
destroys the facet, when the last `std::locale` object holding it is destroyed.
Otherwise, the object is not destroyed.

### Parameters

- **name** — the name of the locale
- **refs** — the number of references that link to the facet

## std::ctype_byname::~ctype_byname

```cpp
protected:
~ctype_byname();
```

Destroys the facet.

## Inherited from std::ctype<CharT>

### Member types

- **`char_type`** — `CharT`

### Member objects

- **static std::locale::id id [static]** — *id* of the locale (public static
  member constant)

**if `CharT` is char, the following member of `std::ctype<char>` is inherited**

- **static const std::size_t table_size [static]** — size of the classification
  table, at least 256 (public static member constant)

### Member functions

- **is** — invokes `do_is` (public member function of `std::ctype<CharT>`)
- **scan_is** — invokes `do_scan_is` (public member function of
  `std::ctype<CharT>`)
- **scan_not** — invokes `do_scan_not` (public member function of
  `std::ctype<CharT>`)
- **toupper** — invokes `do_toupper` (public member function of
  `std::ctype<CharT>`)
- **tolower** — invokes `do_tolower` (public member function of
  `std::ctype<CharT>`)
- **widen** — invokes `do_widen` (public member function of `std::ctype<CharT>`)
- **narrow** — invokes `do_narrow` (public member function of
  `std::ctype<CharT>`)

**if `CharT` is char, the following members of `std::ctype<char>` are inherited**

- **table** — obtains the character classification table (public member function
  of `std::ctype<char>`)
- **classic_table [static]** — obtains the "C" locale character classification
  table (public static member function of `std::ctype<char>`)

### Protected member functions

- **do_toupper [virtual]** — converts a character or characters to uppercase
  (virtual protected member function of `std::ctype<CharT>`)
- **do_tolower [virtual]** — converts a character or characters to lowercase
  (virtual protected member function of `std::ctype<CharT>`)
- **do_widen [virtual]** — converts a character or characters from char to
  `CharT` (virtual protected member function of `std::ctype<CharT>`)
- **do_narrow [virtual]** — converts a character or characters from `CharT` to
  char (virtual protected member function of `std::ctype<CharT>`)

**if `CharT` is char, the following members of `std::ctype` are NOT inherited**

- **do_is [virtual]** — classifies a character or a character sequence (virtual
  protected member function of `std::ctype<CharT>`)
- **do_scan_is [virtual]** — locates the first character in a sequence that
  conforms to given classification (virtual protected member function of
  `std::ctype<CharT>`)
- **do_scan_not [virtual]** — locates the first character in a sequence that
  fails given classification (virtual protected member function of
  `std::ctype<CharT>`)

## Inherited from std::ctype_base

### Member types

- **`mask`** — unspecified bitmask type (enumeration, integer type, or bitset)

### Member constants

- **space [static]** — the value of `mask` identifying whitespace character
  classification (public static member constant)
- **print [static]** — the value of `mask` identifying printable character
  classification (public static member constant)
- **cntrl [static]** — the value of `mask` identifying control character
  classification (public static member constant)
- **upper [static]** — the value of `mask` identifying uppercase character
  classification (public static member constant)
- **lower [static]** — the value of `mask` identifying lowercase character
  classification (public static member constant)
- **alpha [static]** — the value of `mask` identifying alphabetic character
  classification (public static member constant)
- **digit [static]** — the value of `mask` identifying digit character
  classification (public static member constant)
- **punct [static]** — the value of `mask` identifying punctuation character
  classification (public static member constant)
- **xdigit [static]** — the value of `mask` identifying hexadecimal digit
  character classification (public static member constant)
- **blank [static] (C++11)** — the value of `mask` identifying blank character
  classification (public static member constant)
- **alnum [static]** — `alpha | digit` (public static member constant)
- **graph [static]** — `alnum | punct` (public static member constant)

### Notes

The explicit specialization `std::ctype_byname<char>` was listed as a separate
entry in the header file `<locale>` until C++11. it was removed as LWG issue
1298, but it remains a required specialization, just like
std::ctype_byname<wchar_t>.

### Example

```cpp
#include <iostream>
#include <locale>

int main()
{
    wchar_t c = L'\u00de'; // capital letter thorn

    std::locale loc("C");

    std::cout << "isupper('Þ', C locale) returned "
              << std::boolalpha << std::isupper(c, loc) << '\n';

    loc = std::locale(loc, new std::ctype_byname<wchar_t>("en_US.utf8"));

    std::cout << "isupper('Þ', C locale with Unicode ctype) returned "
              << std::boolalpha << std::isupper(c, loc) << '\n';
}
```

Output:

```text
isupper('Þ', C locale) returned false
isupper('Þ', C locale with Unicode ctype) returned true
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 16 | C++98 | the definition of the explicit specialization
      std::ctype_byname<char> misspecified the name and parameter list of
      `do_narrow` | corrected
  LWG 616 | C++98 | the typename disambiguator was missing in the definition of
      `mask` | added

### See also

- **ctype** — defines character classification tables (class template)
- **ctype<char>** — specialization of `std::ctype` for type char (class template
  specialization)

---
*Source: https://en.cppreference.com/w/cpp/locale/ctype_byname*
