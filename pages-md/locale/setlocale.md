# std::setlocale

```cpp
char* setlocale( int category, const char* locale );
```

The `setlocale` function installs the specified system locale or its portion as
the new C locale. The modifications remain in effect and influences the
execution of all locale-sensitive C library functions until the next call to
`setlocale`. If `locale` is a null pointer, `setlocale` queries the current C
locale without modifying it.

### Parameters

- **category** — locale category identifier, one of the `LC_xxx` macros. May be
  `​0​`.
- **locale** — system-specific locale identifier. Can be `""` for the
  user-preferred locale or `"C"` for the minimal locale

### Return value

Pointer to a narrow null-terminated string identifying the C locale after
applying the changes, if any, or null pointer on failure.

A copy of the returned string along with the category used in this call to
`std::setlocale` may be used later in the program to restore the locale back to
the state at the end of this call.

### Notes

During program startup, the equivalent of `std::setlocale(LC_ALL, "C");` is
executed before any user code is run.

Although the return type is `char*`, modifying the pointed-to characters is
undefined behavior.

Because `setlocale` modifies global state which affects execution of
locale-dependent functions, it is undefined behavior to call it from one thread,
while another thread is executing any of the following functions:
`std::fprintf`, `std::isprint`, `std::iswdigit`, `std::localeconv`,
`std::tolower`, `std::fscanf`, `std::ispunct`, `std::iswgraph`, `std::mblen`,
`std::toupper`, `std::isalnum`, `std::isspace`, `std::iswlower`,
`std::mbstowcs`, `std::towlower`, `std::isalpha`, `std::isupper`,
`std::iswprint`, `std::mbtowc`, `std::towupper`, `std::isblank`,
`std::iswalnum`, `std::iswpunct`, `std::setlocale`, `std::wcscoll`,
`std::iscntrl`, `std::iswalpha`, `std::iswspace`, `std::strcoll`, `std::wcstod`,
`std::isdigit`, `std::iswblank`, `std::iswupper`, `std::strerror`,
`std::wcstombs`, `std::isgraph`, `std::iswcntrl`, `std::iswxdigit`,
`std::strtod`, `std::wcsxfrm`, `std::islower`, `std::iswctype`, `std::isxdigit`.

POSIX also defines a locale named `"POSIX"`, which is always accessible and is
exactly equivalent to the default minimal `"C"` locale.

POSIX also specifies that the returned pointer, not just the contents of the
pointed-to string, may be invalidated by subsequent calls to `setlocale`.

### Example

```cpp
#include <clocale>
#include <cstdio>
#include <ctime>
#include <cwchar>
#include <iterator>
#include <string>

int main()
{
    // Make a "deep copy" of current locale name.
    std::string prev_loc = std::setlocale(LC_ALL, nullptr);

    // The C locale will be UTF-8 enabled English,
    // decimal dot will be German,
    // date and time formatting will be Japanese.
    if (const char* loc = std::setlocale(LC_ALL, "en_US.UTF-8"))
        std::wprintf(L"New LC_ALL locale: %s\n", loc);
    if (const char* loc = std::setlocale(LC_NUMERIC, "de_DE.UTF-8"))
        std::wprintf(L"New LC_NUMERIC locale: %s\n", loc);
    if (const char* loc = std::setlocale(LC_TIME, "ja_JP.UTF-8"))
        std::wprintf(L"New LC_TIME locale: %s\n", loc);

    wchar_t buf[100];
    std::time_t t = std::time(nullptr);
    std::wcsftime(buf, std::size(buf), L"%A %c", std::localtime(&t));
    std::wprintf(L"Number: %.2f\nDate: %Ls\n", 3.14, buf);

    // Restore the previous locale.
    if (const char* loc = std::setlocale(LC_ALL, prev_loc.c_str()))
        std::wprintf(L"Restorred LC_ALL locale: %s\n", loc);
}
```

Possible output:

```text
New LC_ALL locale: en_US.UTF-8
New LC_NUMERIC locale: de_DE.UTF-8
New LC_TIME locale: ja_JP.UTF-8
Number: 3,14
Date: 日曜日 2022年11月06日 20時40分59秒
Restorred LC_ALL locale: C
```

### See also

- **LC_ALLLC_COLLATELC_CTYPELC_MONETARYLC_NUMERICLC_TIME** — locale categories
  for `std::setlocale` (macro constant)
- **locale** — set of polymorphic facets that encapsulate cultural differences
  (class)

**C documentation for `setlocale`**

---
*Source: https://en.cppreference.com/w/cpp/locale/setlocale*
