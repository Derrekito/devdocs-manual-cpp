# std::money_put

```cpp
template<
    class CharT,
    class OutputIt = std::ostreambuf_iterator<CharT>
> class money_put;
```

Class `std::money_put` encapsulates the rules for formatting monetary values as
strings. The standard I/O manipulator `std::put_money` uses the `std::money_put`
facet of the I/O stream's locale.

If a `std::money_put` specialization is not guaranteed to be provided by the
standard library (see below), the behaviors of its `put()` and `do_put()` are
not guaranteed as specified.

### Specializations

The standard library is guaranteed to provide the following specializations
(they are required to be implemented by any locale object):

- **std::money_put<char>** — creates narrow string representations of monetary
  values
- **std::money_put<wchar_t>** — creates wide string representations of monetary
  values

In addition, the standard library is also guaranteed to provide every
specialization that satisfies the following type requirements:

- `CharT` is one of char, wchar_t, and any other implementation-defined
  character type that meets the requirements for a character on which any of the
  iostream components can be instantiated; and
- `OutputIt` must meet the requirements of LegacyOutputIterator.

### Member types

- **`char_type`** — `CharT`
- **`string_type`** — std::basic_string<CharT>
- **`iter_type`** — `OutputIt`

### Member functions

- **(constructor)** — constructs a new `money_put` facet (public member
  function)
- **put** — invokes `do_put` (public member function)

### Protected member functions

- **(destructor)** — destructs a `money_put` facet (protected member function)
- **do_put [virtual]** — formats a monetary value and writes to output stream
  (virtual protected member function)

### Member objects

- **static std::locale::id id** — *id* of the locale (public member object)

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <iterator>
#include <locale>

int main()
{
    // using the I/O manipulator
    std::cout.imbue(std::locale("en_US.UTF-8"));
    std::cout << "American locale: "
              << std::showbase << std::put_money(12345678.9) << '\n';

    // using the facet directly
    std::cout.imbue(std::locale("de_DE.UTF-8"));
    std::cout << "German locale: ";
    auto& f = std::use_facet<std::money_put<char>>(std::cout.getloc());
    f.put({std::cout}, false, std::cout, std::cout.fill(), 12345678.9);
    std::cout << '\n';
}
```

Output:

```text
American locale: $123,456.79
German locale: 123.456,79 €
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 427 | C++98 | `money_put` could accept any `CharT` that meets the
      requirements for a character on which any of the iostream components can
      be instantiated | only guarantees to accept char and wchar_t, other types
      are implementation-defined

### See also

- **moneypunct** — defines monetary formatting parameters used by
  `std::money_get` and `std::money_put` (class template)
- **money_get** — parses and constructs a monetary value from an input character
  sequence (class template)
- **put_money (C++11)** — formats and outputs a monetary value (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/locale/money_put*
