# LC_ALL, LC_COLLATE, LC_CTYPE, LC_MONETARY, LC_NUMERIC, LC_TIME

```cpp
#define LC_ALL      /* implementation defined */
#define LC_COLLATE  /* implementation defined */
#define LC_CTYPE    /* implementation defined */
#define LC_MONETARY /* implementation defined */
#define LC_NUMERIC  /* implementation defined */
#define LC_TIME     /* implementation defined */
```

Each of the above macro constants expand to integer constant expressions with
distinct values that are suitable for use as the first argument of
`std::setlocale`.

- **`LC_ALL`** — selects the entire C locale
- **`LC_COLLATE`** — selects the collation category of the C locale
- **`LC_CTYPE`** — selects the character classification category of the C locale
- **`LC_MONETARY`** — selects the monetary formatting category of the C locale
- **`LC_NUMERIC`** — selects the numeric formatting category of the C locale
- **`LC_TIME`** — selects the time formatting category of the C locale

Additional macro constants, with names that begin with `LC_` followed by at
least one uppercase letter, may be defined in `<clocale>`. For example, the
POSIX specification requires `LC_MESSAGES` (which controls `std::perror` and
`std::strerror`), ISO/IEC 30112:2014 (2014 draft) additionally defines
`LC_IDENTIFICATION`, `LC_XLITERATE`, `LC_NAME`, `LC_ADDRESS`, `LC_TELEPHONE`,
`LC_PAPER`, `LC_MEASUREMENT`, and `LC_KEYBOARD`, which are supported by the GNU
C library (except for `LC_XLITERATE`).

### Example

```cpp
#include <clocale>
#include <cstdio>
#include <ctime>
#include <cwchar>

int main()
{
    // the C locale will be the UTF-8 enabled English:
    std::setlocale(LC_ALL, "en_US.UTF-8");

    // decimal dot will be German:
    std::setlocale(LC_NUMERIC, "de_DE.UTF-8");

    // date/time formatting will be Japanese:
    std::setlocale(LC_TIME, "ja_JP.UTF-8");

    wchar_t str[100];
    std::time_t t = std::time(nullptr);
    std::wcsftime(str, 100, L"%A %c", std::localtime(&t));
    std::wprintf(L"Number: %.2f\nDate: %Ls\n", 3.14, str);
}
```

Output:

```text
Number: 3,14
Date: 日曜日 2022年11月06日 17時55分10秒
```

### See also

- **setlocale** — gets and sets the current C locale (function)
- **locale** — set of polymorphic facets that encapsulate cultural differences
  (class)

**C documentation for locale categories**

---
*Source: https://en.cppreference.com/w/cpp/locale/LC_categories*
