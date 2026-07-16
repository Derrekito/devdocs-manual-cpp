# std::messages<CharT>::open, std::messages<CharT>::do_open

```cpp
public:
catalog open( const std::basic_string<char>& name, const std::locale& loc ) const;  // (1)
protected:
virtual catalog do_open( const std::basic_string<char>& name, const std::locale& loc ) const;  // (2)
```

1) Public member function, calls the protected virtual member function `do_open`
   of the most derived class.

2) Obtains a value of type `catalog` (inherited from `std::messages_base`),
   which can be passed to `get()` to retrieve messages from the message catalog
   named by `name`. This value is usable until passed to `close()`.

### Parameters

- **name** — name of the message catalog to open
- **loc** — a locale object that provides additional facets that may be required
  to read messages from the catalog, such as `std::codecvt` to perform
  wide/multibyte conversions

### Return value

The non-negative value of type `catalog` that can be used with `get()` and
`close()`. Returns a negative value if the catalog could not be opened.

### Notes

On POSIX systems, this function call usually translates to a call to
`catopen()`. In GNU libstdc++, it calls `textdomain`.

The actual catalog location is implementation-defined: for the catalog `"sed"`
(message catalogs installed with the Unix utility `'sed'`) in German locale, for
example, the file opened by this function call may be
`/usr/lib/nls/msg/de_DE/sed.cat`, `/usr/lib/locale/de_DE/LC_MESSAGES/sed.cat`,
or `/usr/share/locale/de/LC_MESSAGES/sed.mo`.

### Example

The following example demonstrated retrieval of messages: on a typical GNU/Linux
system it reads from `/usr/share/locale/de/LC_MESSAGES/sed.mo`.

```cpp
#include <iostream>
#include <locale>

int main()
{
    std::locale loc("de_DE.utf8");
    std::cout.imbue(loc);
    auto& facet = std::use_facet<std::messages<char>>(loc);
    auto cat = facet.open("sed", loc);
    if (cat < 0)
        std::cout << "Could not open german \"sed\" message catalog\n";
    else
        std::cout << "\"No match\" in German: "
                  << facet.get(cat, 0, 0, "No match") << '\n'
                  << "\"Memory exhausted\" in German: "
                  << facet.get(cat, 0, 0, "Memory exhausted") << '\n';
    facet.close(cat);
}
```

Possible output:

```text
"No match" in German: Keine Übereinstimmung
"Memory exhausted" in German: Speicher erschöpft
```

### See also

---
*Source: https://en.cppreference.com/w/cpp/locale/messages/open*
