# std::moneypunct_byname

```cpp
template< class CharT, bool Intl = false >
class moneypunct_byname : public std::moneypunct<CharT, Intl>;
```

`std::moneypunct_byname` is a `std::moneypunct` facet which encapsulates
monetary formatting preferences of a locale specified at its construction.

### Specializations

The standard library is guaranteed to provide every specialization that
satisfies the following type requirements:

- `CharT` is one of char and wchar_t, and
- `Intl` is a possible specialization on a bool parameter.

### Member types

- **`pattern`** — `std::money_base::pattern`
- **`string_type`** — std::basic_string<CharT>

### Member functions

- ****(constructor)**** — constructs a new `moneypunct_byname` facet (public
  member function)
- ****(destructor)**** — destroys a `moneypunct_byname` facet (protected member
  function)

## std::moneypunct_byname::moneypunct_byname

```cpp
explicit moneypunct_byname( const char* name, std::size_t refs = 0 );
explicit moneypunct_byname( const std::string& name, std::size_t refs = 0 );  // (since C++11)
```

Constructs a new `std::moneypunct_byname` facet for a locale with `name`.

`refs` is used for resource management: if `refs == 0`, the implementation
destroys the facet, when the last `std::locale` object holding it is destroyed.
Otherwise, the object is not destroyed.

### Parameters

- **name** — the name of the locale
- **refs** — the number of references that link to the facet

## std::moneypunct_byname::~moneypunct_byname

```cpp
protected:
~moneypunct_byname();
```

Destroys the facet.

## Inherited from std::moneypunct

### Member types

- **`char_type`** — `CharT`
- **`string_type`** — std::basic_string<CharT>

### Member functions

- **decimal_point** — invokes `do_decimal_point` (public member function of
  `std::moneypunct<CharT,International>`)
- **thousands_sep** — invokes `do_thousands_sep` (public member function of
  `std::moneypunct<CharT,International>`)
- **grouping** — invokes `do_grouping` (public member function of
  `std::moneypunct<CharT,International>`)
- **curr_symbol** — invokes `do_curr_symbol` (public member function of
  `std::moneypunct<CharT,International>`)
- **positive_signnegative_sign** — invokes `do_positive_sign` or
  `do_negative_sign` (public member function of
  `std::moneypunct<CharT,International>`)
- **frac_digits** — invokes `do_frac_digits` (public member function of
  `std::moneypunct<CharT,International>`)
- **pos_formatneg_format** — invokes `do_pos_format`/`do_neg_format` (public
  member function of `std::moneypunct<CharT,International>`)

### Protected member functions

- **do_decimal_point [virtual]** — provides the character to use as decimal
  point (virtual protected member function of
  `std::moneypunct<CharT,International>`)
- **do_thousands_sep [virtual]** — provides the character to use as thousands
  separator (virtual protected member function of
  `std::moneypunct<CharT,International>`)
- **do_grouping [virtual]** — provides the numbers of digits between each pair
  of thousands separators (virtual protected member function of
  `std::moneypunct<CharT,International>`)
- **do_curr_symbol [virtual]** — provides the string to use as the currency
  identifier (virtual protected member function of
  `std::moneypunct<CharT,International>`)
- **do_positive_signdo_negative_sign [virtual]** — provides the string to
  indicate a positive or negative value (virtual protected member function of
  `std::moneypunct<CharT,International>`)
- **do_frac_digits [virtual]** — provides the number of digits to display after
  the decimal point (virtual protected member function of
  `std::moneypunct<CharT,International>`)
- **do_pos_formatdo_neg_format [virtual]** — provides the formatting pattern for
  currency values (virtual protected member function of
  `std::moneypunct<CharT,International>`)

### Member constants

- **const bool `intl` (static)** — `International`

### Member objects

- **static std::locale::id id** — *id* of the locale (public member object)

## Inherited from std::money_base

- **enum part { none, space, symbol, sign, value };** — unscoped enumeration
  type
- **struct pattern { char field[4]; };** — the monetary format type

- **`none`** — whitespace is permitted but not required except in the last
  position, where whitespace is not permitted
- **`space`** — one or more whitespace characters are required
- **`symbol`** — the sequence of characters returned by
  `std::moneypunct::curr_symbol` is required
- **`sign`** — the first of the characters returned by
  `std::moneypunct::positive_sign` or `std::moneypunct::negative_sign` is
  required
- **`value`** — the absolute numeric monetary value is required

### Example

This example demonstrates how to apply monetary formatting rules of another
language without changing the rest of the locale.

```cpp
#include <iomanip>
#include <iostream>
#include <locale>

int main()
{
    long double mon = 1234567;
    std::locale::global(std::locale("en_US.utf8"));
    std::wcout.imbue(std::locale());
    std::wcout << L"american locale: " << std::showbase
               << std::put_money(mon) << '\n';
    std::wcout.imbue(std::locale(std::wcout.getloc(),
                                 new std::moneypunct_byname<wchar_t>("ru_RU.utf8")));
    std::wcout << L"american locale with russian moneypunct: "
               << std::put_money(mon) << '\n';
}
```

Output:

```text
american locale: $12,345.67
american locale with russian moneypunct: 12 345.67 руб
```

### See also

- **moneypunct** — defines monetary formatting parameters used by
  `std::money_get` and `std::money_put` (class template)

---
*Source: https://en.cppreference.com/w/cpp/locale/moneypunct_byname*
