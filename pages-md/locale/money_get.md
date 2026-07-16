# std::money_get

```cpp
template<
    class CharT,
    class InputIt = std::istreambuf_iterator<CharT>
> class money_get;
```

Class template `std::money_get` encapsulates the rules for parsing monetary
values from character streams. The standard I/O manipulator `std::get_money`
uses the `std::money_get` facet of the I/O stream's locale.

If a `std::money_get` specialization is not guaranteed to be provided by the
standard library (see below), the behaviors of its `get()` and `do_get()` are
not guaranteed as specified.

### Specializations

The standard library is guaranteed to provide the following specializations
(they are required to be implemented by any locale object):

- **std::money_get<char>** — parses narrow string representations of monetary
  values
- **std::money_get<wchar_t>** — parses wide string representations of monetary
  values

In addition, the standard library is also guaranteed to provide every
specialization that satisfies the following type requirements:

- `CharT` is one of char, wchar_t, and any other implementation-defined
  character type that meets the requirements for a character on which any of the
  iostream components can be instantiated; and
- `InputIt` must meet the requirements of LegacyInputIterator.

### Member types

- **`char_type`** — `CharT`
- **`string_type`** — std::basic_string<CharT>
- **`iter_type`** — `InputIt`

### Member functions

- **(constructor)** — constructs a new `money_get` facet (public member
  function)
- **get** — invokes `do_get` (public member function)

### Protected member functions

- **(destructor)** — destructs a `money_get` facet (protected member function)
- **do_get [virtual]** — parses a monetary value from an input stream (virtual
  protected member function)

### Member objects

- **static std::locale::id id** — *id* of the locale (public member object)

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <iterator>
#include <locale>
#include <sstream>

int main()
{
    std::string str = "$1.11 $2.22 $3.33";
    std::cout << std::fixed << std::setprecision(2);

    std::cout << '\"' << str << "\" parsed with the I/O manipulator: ";
    std::istringstream s1(str);
    s1.imbue(std::locale("en_US.UTF-8"));

    long double val;
    while (s1 >> std::get_money(val))
        std::cout << val / 100 << ' ';
    std::cout << '\n';

    str = "USD  1,234.56";
    std::cout << '\"' << str << "\" parsed with the facet directly: ";
    std::istringstream s2(str);
    s2.imbue(std::locale("en_US.UTF-8"));

    auto& f = std::use_facet<std::money_get<char>>(s2.getloc());
    std::ios_base::iostate err;
    std::istreambuf_iterator<char> beg(s2), end;
    f.get(beg, end, true, s2, err, val);

    std::cout << val / 100 << '\n';
}
```

Output:

```text
"$1.11 $2.22 $3.33" parsed with the I/O manipulator: 1.11 2.22 3.33
"USD  1,234.56" parsed with the facet directly: 1234.56
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 427 | C++98 | `money_get` could accept any `CharT` that meets the
      requirements for a character on which any of the iostream components can
      be instantiated | only guarantees to accept char and wchar_t, other types
      are implementation-defined

### See also

- **moneypunct** — defines monetary formatting parameters used by
  `std::money_get` and `std::money_put` (class template)
- **money_put** — formats a monetary value for output as a character sequence
  (class template)
- **get_money (C++11)** — parses a monetary value (function template)

---
*Source: https://en.cppreference.com/w/cpp/locale/money_get*
