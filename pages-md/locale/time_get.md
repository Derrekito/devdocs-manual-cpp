# std::time_get

```cpp
template<
    class CharT,
    class InputIt = std::istreambuf_iterator<CharT>
> class time_get;
```

Class template `std::time_get` encapsulates date and time parsing rules. The I/O
manipulator `std::get_time` uses the `std::time_get` facet of the I/O stream's
locale to convert text input to a `std::tm` object.

If a `std::time_get` specialization is not guaranteed to be provided by the
standard library (see below), the behaviors of its member functions (except the
constructor and destructor) are not guaranteed as specified.

### Specializations

The standard library is guaranteed to provide the following specializations
(they are required to be implemented by any locale object):

- **std::time_get<char>** — parses narrow string representations of date and
  time
- **std::time_get<wchar_t>** — parses wide string representations of date and
  time

In addition, the standard library is also guaranteed to provide every
specialization that satisfies the following type requirements:

- `CharT` is one of char and wchar_t, and
- `InputIt` must meet the requirements of LegacyInputIterator.

### Member types

- **`char_type`** — `CharT`
- **`iter_type`** — `InputIt`

### Member functions

- **(constructor)** — constructs a new `time_get` facet (public member function)
- **(destructor)** — destructs a `time_get` facet (protected member function)
- **date_order** — invokes `do_date_order` (public member function)
- **get_time** — invokes `do_get_time` (public member function)
- **get_date** — invokes `do_get_date` (public member function)
- **get_weekday** — invokes `do_get_weekday` (public member function)
- **get_monthname** — invokes `do_get_monthname` (public member function)
- **get_year** — invokes `do_get_year` (public member function)
- **get (C++11)** — invokes `do_get` (public member function)

### Member objects

- **static std::locale::id id** — *id* of the locale (public member object)

### Protected member functions

- **do_date_order [virtual]** — obtains preferred ordering of day, month, and
  year (virtual protected member function)
- **do_get_time [virtual]** — extracts hours, minutes, and seconds from input
  stream (virtual protected member function)
- **do_get_date [virtual]** — extracts month, day, and year from input stream
  (virtual protected member function)
- **do_get_weekday [virtual]** — extracts the name of a day of the week from
  input stream (virtual protected member function)
- **do_get_monthname [virtual]** — extracts a month name from input stream
  (virtual protected member function)
- **do_get_year [virtual]** — extracts a year from input stream (virtual
  protected member function)
- **do_get [virtual] (C++11)** — extracts date/time components from input
  stream, according to the specified format (virtual protected member function)

## Inherited from std::time_base

- **`dateorder`** — date order enumeration type, defining the values `no_order`,
  `dmy`, `mdy`, `ymd`, and `ydm`

### Example

Note: choose clang to observe the output. libstdc++ does not correctly implement
the %b specifier: bug 78714.

```cpp
#include <iomanip>
#include <iostream>
#include <locale>
#include <sstream>

int main()
{
    std::tm t = {};
    std::istringstream ss("2011-Februar-18 23:12:34");
    ss.imbue(std::locale("de_DE.utf-8"));

    ss >> std::get_time(&t, "%Y-%b-%d %H:%M:%S");
    if (ss.fail())
        std::cout << "Parse failed\n";
    else
        std::cout << std::put_time(&t, "%c") << '\n';
}
```

Possible output:

```text
Sun Feb 18 23:12:34 2011
```

### See also

- **time_put** — formats contents of `std::tm` for output as character sequence
  (class template)
- **get_time (C++11)** — parses a date/time value of specified format (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/locale/time_get*
