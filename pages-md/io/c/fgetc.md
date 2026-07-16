# std::fgetc, std::getc

```cpp
int fgetc( std::FILE* stream );
int getc( std::FILE* stream );
```

Reads the next character from the given input stream.

### Parameters

- **stream** — to read the character from

### Return value

The obtained character on success or `EOF` on failure.

If the failure has been caused by end of file condition, additionally sets the
*eof* indicator (see `std::feof()`) on `stream`. If the failure has been caused
by some other error, sets the *error* indicator (see `std::ferror()`) on
`stream`.

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

- **gets (deprecated in C++11)(removed in C++14)** — reads a character string
  from `stdin` (function)
- **fputcputc** — writes a character to a file stream (function)
- **ungetc** — puts a character back into a file stream (function)

**C documentation for `fgetc, getc`**

---
*Source: https://en.cppreference.com/w/cpp/io/c/fgetc*
