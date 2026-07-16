# std::clearerr

```cpp
void clearerr( std::FILE* stream );
```

Resets the error flags and the `EOF` indicator for the given file stream.

### Parameters

- **stream** — the file to reset the error flags for

### Return value

(none)

### Example

```cpp
#include <cassert>
#include <cstdio>

int main()
{
    std::FILE* tmpf = std::tmpfile();
    std::fputs("cppreference.com\n", tmpf);
    std::rewind(tmpf);

    for (int ch; (ch = std::fgetc(tmpf)) != EOF; std::putchar(ch)) { }

    assert(std::feof(tmpf)); // the loop is expected to terminate by EOF
    std::puts("End of file reached");

    std::clearerr(tmpf); // clear EOF

    std::puts(std::feof(tmpf) ? "EOF indicator set"
                              : "EOF indicator cleared");
}
```

Output:

```text
cppreference.com
End of file reached
EOF indicator cleared
```

### See also

- **feof** — checks for the end-of-file (function)
- **perror** — displays a character string corresponding of the current error to
  `stderr` (function)
- **ferror** — checks for a file error (function)

**C documentation for `clearerr`**

---
*Source: https://en.cppreference.com/w/cpp/io/c/clearerr*
