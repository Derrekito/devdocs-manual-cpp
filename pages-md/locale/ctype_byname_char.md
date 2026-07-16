# std::ctype_byname<char>

```cpp
template<>
class ctype_byname : public std::ctype<char>;
```

This specialization of `std::ctype_byname` encapsulates character classification
features for type `char`. Like its base class `std::ctype<char>` and unlike
general-purpose `std::ctype_byname`, table lookup is used to classify characters

### Member types

- **`mask`** — `ctype<char>::mask`

### Member functions

- ****(constructor)**** — constructs a new ctype_byname<char> facet (public
  member function)
- ****(destructor)**** — destructs a ctype_byname<char> facet (protected member
  function)

## Inherited from std::ctype<char>

### Member types

- **`char_type`** — `char`

### Member objects

- **`id` (static)** — `std::locale::id`
- **`table_size` (static const)** — `std::size_t` size of the classification
  table, at least 256

### Member functions

- **table** — obtains the character classification table (public member function
  of `std::ctype<char>`)
- **classic_table [static]** — obtains the "C" locale character classification
  table (public static member function of `std::ctype<char>`)
- **is** — classifies a character or a character sequence, using the
  classification table (public member function of `std::ctype<char>`)
- **scan_is** — locates the first character in a sequence that conforms to given
  classification, using the classification table (public member function of
  `std::ctype<char>`)
- **scan_not** — locates the first character in a sequence that fails given
  classification, using the classification table (public member function of
  `std::ctype<char>`)
- **toupper** — invokes `do_toupper` (public member function of
  `std::ctype<CharT>`)
- **tolower** — invokes `do_tolower` (public member function of
  `std::ctype<CharT>`)
- **widen** — invokes `do_widen` (public member function of `std::ctype<CharT>`)
- **narrow** — invokes `do_narrow` (public member function of
  `std::ctype<CharT>`)

### Protected member functions

- **do_toupper [virtual]** — converts a character or characters to uppercase
  (virtual protected member function of `std::ctype<CharT>`)
- **do_tolower [virtual]** — converts a character or characters to lowercase
  (virtual protected member function of `std::ctype<CharT>`)
- **do_widen [virtual]** — converts a character or characters from char to
  `CharT` (virtual protected member function of `std::ctype<CharT>`)
- **do_narrow [virtual]** — converts a character or characters from `CharT` to
  char (virtual protected member function of `std::ctype<CharT>`)

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

### Example

```cpp
#include <iostream>
#include <locale>

int main()
{
    char c = '\xde'; // capital letter thorn

    std::locale loc("C");

    std::cout << "isupper('Þ', C locale) returned "
               << std::boolalpha << std::isupper(c, loc) << '\n';

    loc = std::locale(loc, new std::ctype_byname<char>("en_US.utf8"));

    std::cout << "isupper('Þ', C locale with Unicode ctype<char>) returned "
              << std::boolalpha << std::isupper(c, loc) << '\n';

    loc = std::locale(loc, new std::ctype_byname<char>("is_IS.iso88591"));

    std::cout << "isupper('Þ', C locale with Islandic ctype<char>) returned "
              << std::boolalpha << std::isupper(c, loc) << '\n';
}
```

Output:

```text
isupper('Þ', C locale) returned false
isupper('Þ', C locale with Unicode ctype<char>) returned false
isupper('Þ', C locale with Islandic ctype<char>) returned true
```

### See also

- **ctype** — defines character classification tables (class template)
- **ctype<char>** — specialization of `std::ctype` for type char (class template
  specialization)

---
*Source: https://en.cppreference.com/w/cpp/locale/ctype_byname_char*
