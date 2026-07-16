# std::fputwc

```cpp
std::wint_t fputwc( wchar_t ch, std::FILE* stream );  // (1)
std::wint_t putwc( wchar_t ch, std::FILE* stream );  // (2)
```

Writes a wide character `ch` to the given output stream `stream`.

2) May be implemented as a macro and may evaluate `stream` more than once.

### Parameters

- **ch** — wide character to be written
- **stream** — the output stream

### Return value

`ch` on success, `WEOF` on failure. If an encoding error occurs, `errno` is set
to `EILSEQ`.

### Example

```cpp
#include <cerrno>
#include <clocale>
#include <cstdio>
#include <cstdlib>
#include <cwchar>
#include <initializer_list>

int main()
{
    std::setlocale(LC_ALL, "en_US.utf8");

    for (const wchar_t ch :
    {
        L'\u2200', // Unicode name: "FOR ALL"
        L'\n',
        L'∀',
    })
    {
        if (errno = 0; std::fputwc(ch, stdout) == WEOF)
        {
            std::puts(errno == EILSEQ
                ? "Encoding error in fputwc"
                : "I/O error in fputwc"
            );
            return EXIT_FAILURE;
        }
    }
    return EXIT_SUCCESS;
}
```

Possible output:

```text
∀
∀
```

### See also

- **fputcputc** — writes a character to a file stream (function)
- **fputws** — writes a wide string to a file stream (function)
- **fgetwcgetwc** — gets a wide character from a file stream (function)

**C documentation for `fputwc`**

---
*Source: https://en.cppreference.com/w/cpp/io/c/fputwc*
