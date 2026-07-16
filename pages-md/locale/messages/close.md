# std::messages<CharT>::close, std::messages<CharT>::do_close

```cpp
public:
void close( catalog c ) const;  // (1)
protected:
virtual void do_close( catalog c ) const;  // (2)
```

1) Public member function, calls the protected virtual member function
   `do_close` of the most derived class.

2) Releases the implementation-defined resources associated with an open catalog
   that is designated by the value `c` of type `catalog` (inherited from
   `std::messages_base`), which was obtained from `open()`.

### Parameters

- **c** — a valid open catalog identifier, on which `close()` has not yet been
  called

### Return value

(none)

### Notes

On POSIX systems, this function call usually translates to a call to
`catclose()`. In GNU libstdc++, which is implemented in terms of GNU
`gettext()`, it does nothing.

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
*Source: https://en.cppreference.com/w/cpp/locale/messages/close*
