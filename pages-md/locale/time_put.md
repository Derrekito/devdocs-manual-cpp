# std::time_put

```cpp
template<
    class CharT,
    class OutputIt = std::ostreambuf_iterator<CharT>
> class time_put;
```

Class template `std::time_put` encapsulates date and time formatting rules. The
I/O manipulator `std::put_time` uses the `std::time_put` facet of the I/O
stream's locale to generate text representation of an `std::tm` object.

If a `std::time_put` specialization is not guaranteed to be provided by the
standard library (see below), the behaviors of its `put()` and `do_put()` are
not guaranteed as specified.

### Specializations

The standard library is guaranteed to provide the following specializations
(they are required to be implemented by any locale object):

- **std::time_put<char>** — creates narrow string representations of date and
  time
- **std::time_put<wchar_t>** — creates wide string representations of date and
  time

In addition, the standard library is also guaranteed to provide every
specialization that satisfies the following type requirements:

- `CharT` is one of char and wchar_t, and
- `OutputIt` must meet the requirements of LegacyOutputIterator.

### Member types

- **`char_type`** — `CharT`
- **`iter_type`** — `OutputIt`

### Member functions

- **(constructor)** — constructs a new `time_put` facet (public member function)
- **(destructor)** — destructs a `time_put` facet (protected member function)
- **put** — invokes `do_put` (public member function)

### Member objects

- **static std::locale::id id** — *id* of the locale (public member object)

### Protected member functions

- **do_put [virtual]** — formats date/time and writes to output stream (virtual
  protected member function)

### Example

```cpp
#include <codecvt>
#include <ctime>
#include <iomanip>
#include <iostream>

int main()
{
    std::time_t t = std::time(nullptr);
    std::wbuffer_convert<std::codecvt_utf8<wchar_t>> conv(std::cout.rdbuf());
    std::wostream out(&conv);
    out.imbue(std::locale("ja_JP.utf8"));

    // this I/O manipulator std::put_time uses std::time_put<wchar_t>
    out << std::put_time(std::localtime(&t), L"%A %c") << '\n';
}
```

Output:

```text
水曜日 2011年11月09日 12時32分05秒
```

### See also

- **time_put_byname** — represents the system-supplied `std::time_put` for the
  named locale (class template)
- **time_get** — parses time/date values from an input character sequence into
  `std::tm` (class template)
- **put_time (C++11)** — formats and outputs a date/time value according to the
  specified format (function template)

---
*Source: https://en.cppreference.com/w/cpp/locale/time_put*
