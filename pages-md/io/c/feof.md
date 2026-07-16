# std::feof

```cpp
int feof( std::FILE* stream );
```

Checks if the end of the given file stream has been reached.

### Parameters

- **stream** — the file stream to check

### Return value

Nonzero value if the end of the stream has been reached, otherwise `​0​`.

### Notes

This function only reports the stream state as reported by the most recent I/O
operation, it does not examine the associated data source. For example, if the
most recent I/O was a `std::fgetc`, which returned the last byte of a file,
`std::feof` returns zero. The next `std::fgetc` fails and changes the stream
state to *end-of-file*. Only then `std::feof` returns non-zero.

In typical usage, input stream processing stops on any error; `feof` and
`std::ferror` are then used to distinguish between different error conditions.

### Example

```cpp
#include <cstdio>
#include <cstdlib>

int main()
{
    int is_ok = EXIT_FAILURE;
    FILE* fp = std::fopen("/tmp/test.txt", "w+");
    if (!fp)
    {
        std::perror("File opening failed");
        return is_ok;
    }

    int c; // Note: int, not char, required to handle EOF
    while ((c = std::fgetc(fp)) != EOF) // Standard C I/O file reading loop
        std::putchar(c);

    if (std::ferror(fp))
        std::puts("I/O error when reading");
    else if (std::feof(fp))
    {
        std::puts("End of file reached successfully");
        is_ok = EXIT_SUCCESS;
    }

    std::fclose(fp);
    return is_ok;
}
```

Output:

```text
End of file reached successfully
```

### See also

- **eof** — checks if end-of-file has been reached (public member function of
  `std::basic_ios<CharT,Traits>`)
- **clearerr** — clears errors (function)
- **perror** — displays a character string corresponding of the current error to
  `stderr` (function)
- **ferror** — checks for a file error (function)

**C documentation for `feof`**

---
*Source: https://en.cppreference.com/w/cpp/io/c/feof*
