# std::collate

```cpp
template< class CharT >
class collate;
```

Class `std::collate` encapsulates locale-specific collation (comparison) and
hashing of strings. This facet is used by `std::basic_regex` and can be applied,
by means of `std::locale::operator()`, directly to all standard algorithms that
expect a string comparison predicate.

### Specializations

The standard library is guaranteed to provide the following specializations
(they are required to be implemented by any locale object):

- **std::collate<char>** — implements lexicographical ordering of byte strings
- **std::collate<wchar_t>** — implements lexicographical ordering of wide
  strings

### Member types

- **`char_type`** — `CharT`
- **`string_type`** — std::basic_string<CharT>

### Member functions

- **(constructor)** — constructs a new `collate` facet (public member function)
- **(destructor)** — destructs a `collate` facet (protected member function)
- **compare** — invokes `do_compare` (public member function)
- **transform** — invokes `do_transform` (public member function)
- **hash** — invokes `do_hash` (public member function)

### Member objects

- **static std::locale::id id** — *id* of the locale (public member object)

### Protected member functions

- **do_compare [virtual]** — compares two strings using this facet's collation
  rules (virtual protected member function)
- **do_transform [virtual]** — transforms a string so that collation can be
  replaced by comparison (virtual protected member function)
- **do_hash [virtual]** — generates an integer hash value using this facet's
  collation rules (virtual protected member function)

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <locale>
#include <string>
#include <vector>

int main()
{
    std::locale::global(std::locale("en_US.utf8"));
    std::wcout.imbue(std::locale(""));
    std::vector<std::wstring> v{
        L"ar", L"zebra", L"\u00f6grupp", L"Zebra",
        L"\u00e4ngel",L"\u00e5r", L"f\u00f6rnamn"
    };

    std::wcout << "Default locale collation order: ";
    std::sort(v.begin(), v.end());
    for (auto s : v)
        std::wcout << s << ' ';
    std::wcout << '\n';

    std::wcout << "English locale collation order: ";
    std::sort(v.begin(), v.end(), std::locale("en_US.UTF-8"));
    for (auto s : v)
        std::wcout << s << ' ';
    std::wcout << '\n';

    std::wcout << "Swedish locale collation order: ";
    std::sort(v.begin(), v.end(), std::locale("sv_SE.UTF-8"));
    for (auto s : v)
        std::wcout << s << ' ';
    std::wcout << '\n';
}
```

Output:

```text
Default locale collation order: Zebra ar förnamn zebra ängel år ögrupp
English locale collation order: ängel ar år förnamn ögrupp zebra Zebra
Swedish locale collation order: ar förnamn zebra Zebra år ängel ögrupp
```

### See also

- **operator()** — lexicographically compares two strings using this locale's
  collate facet (public member function of `std::locale`)
- **collate_byname** — represents the system-supplied `std::collate` for the
  named locale (class template)

---
*Source: https://en.cppreference.com/w/cpp/locale/collate*
