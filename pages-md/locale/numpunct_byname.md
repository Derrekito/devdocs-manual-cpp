# std::numpunct_byname

```cpp
template< class CharT >
class numpunct_byname : public std::numpunct<CharT>;
```

`std::numpunct_byname` is a `std::numpunct` facet which encapsulates numeric
punctuation preferences of a locale specified at its construction.

### Specializations

The standard library is guaranteed to provide the following specializations:

- **std::numpunct_byname<char>** — locale-specific `std::numpunct` facet for
  narrow character I/O
- **std::numpunct_byname<wchar_t>** — locale-specific `std::numpunct` facet for
  wide character I/O

### Member types

- **`char_type`** — `CharT`
- **`string_type`** — std::basic_string<CharT>

### Member functions

- ****(constructor)**** — constructs a new `numpunct_byname` facet (public
  member function)
- ****(destructor)**** — destroys a `numpunct_byname` facet (protected member
  function)

## std::numpunct_byname::numpunct_byname

```cpp
explicit numpunct_byname( const char* name, std::size_t refs = 0 );
explicit numpunct_byname( const std::string& name, std::size_t refs = 0 );  // (since C++11)
```

Constructs a new `std::numpunct_byname` facet for a locale with `name`.

`refs` is used for resource management: if `refs == 0`, the implementation
destroys the facet, when the last `std::locale` object holding it is destroyed.
Otherwise, the object is not destroyed.

### Parameters

- **name** — the name of the locale
- **refs** — the number of references that link to the facet

## std::numpunct_byname::~numpunct_byname

```cpp
protected:
~numpunct_byname();
```

Destroys the facet.

## Inherited from std::numpunct

### Member types

- **`char_type`** — `CharT`
- **`string_type`** — std::basic_string<CharT>

### Member functions

- **decimal_point** — invokes `do_decimal_point` (public member function of
  `std::numpunct<CharT>`)
- **thousands_sep** — invokes `do_thousands_sep` (public member function of
  `std::numpunct<CharT>`)
- **grouping** — invokes `do_grouping` (public member function of
  `std::numpunct<CharT>`)
- **truenamefalsename** — invokes `do_truename` or `do_falsename` (public member
  function of `std::numpunct<CharT>`)

### Protected member functions

- **do_decimal_point [virtual]** — provides the character to use as decimal
  point (virtual protected member function of `std::numpunct<CharT>`)
- **do_thousands_sep [virtual]** — provides the character to use as thousands
  separator (virtual protected member function of `std::numpunct<CharT>`)
- **do_grouping [virtual]** — provides the numbers of digits between each pair
  of thousands separators (virtual protected member function of
  `std::numpunct<CharT>`)
- **do_truenamedo_falsename [virtual]** — provides the string to use as the name
  of the boolean `true` and `false` (virtual protected member function of
  `std::numpunct<CharT>`)

### Member objects

- **static std::locale::id id** — *id* of the locale (public member object)

### Example

This example demonstrates how to apply numeric punctuation rules of another
language without changing the rest of the locale.

```cpp
#include <iostream>
#include <locale>

int main()
{
    const double number = 1000.25;
    std::wcout << L"default locale: " << number << L'\n';
    std::wcout.imbue(std::locale(std::wcout.getloc(),
                                 new std::numpunct_byname<wchar_t>("ru_RU.UTF8")));
    std::wcout << L"default locale with russian numpunct: " << number << L'\n';
}
```

Output:

```text
default locale: 1000.25
default locale with russian numpunct: 1 000,25
```

### See also

- **numpunct** — defines numeric punctuation rules (class template)

---
*Source: https://en.cppreference.com/w/cpp/locale/numpunct_byname*
