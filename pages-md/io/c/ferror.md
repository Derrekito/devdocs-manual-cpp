# std::ferror

```cpp
int ferror( std::FILE* stream );
```

Checks the given stream for errors.

### Parameters

- **stream** — the file stream to check

### Return value

Nonzero value if the file stream has errors occurred, `​0​` otherwise.

### Example

```cpp
#include <clocale>
#include <cstdio>
#include <cstdlib>
#include <cwchar>

int main()
{
    const char *fname = std::tmpnam(nullptr);
    std::FILE* f = std::fopen(fname, "wb");
    std::fputs("\xff\xff\n", f); // not a valid UTF-8 character sequence
    std::fclose(f);

    std::setlocale(LC_ALL, "en_US.utf8");
    f = std::fopen(fname, "rb");
    std::wint_t ch;
    while ((ch=std::fgetwc(f)) != WEOF) // attempt to read as UTF-8
        std::printf("%#x ", ch);

    if (std::feof(f))
        puts("EOF indicator set");
    if (std::ferror(f))
        puts("Error indicator set");
}
```

Output:

```text
Error indicator set
```

### See also

- **clearerr** — clears errors (function)
- **feof** — checks for the end-of-file (function)
- **fail** — checks if an error has occurred (public member function of
  `std::basic_ios<CharT,Traits>`)

**C documentation for `ferror`**

---
*Source: https://en.cppreference.com/w/cpp/io/c/ferror*
