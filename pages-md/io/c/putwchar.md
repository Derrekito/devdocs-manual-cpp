# std::putwchar

```cpp
std::wint_t putwchar( wchar_t ch );
```

Writes a wide character `ch` to `stdout`.

### Parameters

- **ch** — wide character to be written

### Return value

`ch` on success, `WEOF` on failure.

### Example

```cpp
#include <clocale>
#include <cstdio>
#include <cstdlib>
#include <cwchar>
#include <initializer_list>

int main()
{
    std::setlocale(LC_ALL, "en_US.utf8");

    for (const wchar_t ch : {
        L'\u2200', // Unicode name: "FOR ALL"
        L'∀',
        L'\n'
        })
        if (std::putwchar(ch) == WEOF)
        {
            std::puts("I/O error in std::putwchar");
            return EXIT_FAILURE;
        }

    return EXIT_SUCCESS;
}
```

Possible output:

```text
∀∀
```

### See also

- **putchar** — writes a character to `stdout` (function)
- **fputwcputwc** — writes a wide character to a file stream (function)

**C documentation for `putwchar`**

---
*Source: https://en.cppreference.com/w/cpp/io/c/putwchar*
