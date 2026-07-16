# std::time_put_byname

```cpp
template<
    class CharT,
    class OutputIt = std::ostreambuf_iterator<CharT>
> class time_put_byname : public std::time_put<CharT, OutputIt>;
```

`std::time_put_byname` is a `std::time_put` facet which encapsulates time and
date formatting rules of the locale specified at its construction.

### Specializations

The standard library is guaranteed to provide every specialization that
satisfies the following type requirements:

- `CharT` is one of char and wchar_t, and
- `OutputIt` must meet the requirements of LegacyOutputIterator.

### Member types

- **`char_type`** — `CharT`
- **`iter_type`** — `OutputIt`

### Member functions

- ****(constructor)**** — constructs a new `time_put_byname` facet (public
  member function)
- ****(destructor)**** — destroys a `time_put_byname` facet (protected member
  function)

## std::time_put_byname::time_put_byname

```cpp
explicit time_put_byname( const char* name, std::size_t refs = 0 );
explicit time_put_byname( const std::string& name, std::size_t refs = 0 );  // (since C++11)
```

Constructs a new `std::time_put_byname` facet for a locale with `name`.

`refs` is used for resource management: if `refs == 0`, the implementation
destroys the facet, when the last `std::locale` object holding it is destroyed.
Otherwise, the object is not destroyed.

### Parameters

- **name** — the name of the locale
- **refs** — the number of references that link to the facet

## std::time_put_byname::~time_put_byname

```cpp
protected:
~time_put_byname();
```

Destroys the facet.

## Inherited from std::time_put

### Member objects

- **`id` (static)** — `std::locale::id`

### Member functions

- **put** — invokes `do_put` (public member function of
  `std::time_put<CharT,OutputIt>`)

### Protected member functions

- **do_put [virtual]** — formats date/time and writes to output stream (virtual
  protected member function of `std::time_put<CharT,OutputIt>`)

### Example

Prints current time using the "C" locale with the `time_put` facet replaced by
various `std::time_put_byname` facets. The result shown was obtained using the
clang compiler.

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

    out.imbue(std::locale(out.getloc(),
                          new std::time_put_byname<wchar_t>("ja_JP.utf8")));
    out << std::put_time(std::localtime(&t), L"%A %c") << '\n';

    out.imbue(std::locale(out.getloc(),
                          new std::time_put_byname<wchar_t>("ru_RU.utf8")));
    out << std::put_time(std::localtime(&t), L"%A %c") << '\n';

    out.imbue(std::locale(out.getloc(),
                          new std::time_put_byname<wchar_t>("sv_SE.utf8")));
    out << std::put_time(std::localtime(&t), L"%A %c") << '\n';
}
```

Possible output:

```text
木曜日 2023年10月05日 19時44分51秒
Четверг Чт 05 окт 2023 19:44:51
torsdag tor  5 okt 2023 19:44:51
```

### See also

- **time_put** — formats contents of `std::tm` for output as character sequence
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/locale/time_put_byname*
