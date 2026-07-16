# std::fclose

```cpp
int fclose( std::FILE* stream );
```

Closes the given file stream. Any unwritten buffered data are flushed to the OS.
Any unread buffered data are discarded.

Whether or not the operation succeeds, the stream is no longer associated with a
file, and the buffer allocated by `std::setbuf` or `std::setvbuf`, if any, is
also disassociated and deallocated if automatic allocation was used.

The behavior is undefined if the value of the pointer `stream` is used after
`fclose` returns.

### Parameters

- **stream** — the file stream to close

### Return value

`​0​` on success, `EOF` otherwise.

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

- **fopen** — opens a file (function)
- **freopen** — open an existing stream with a different name (function)
- **close** — flushes the put area buffer and closes the associated file (public
  member function of `std::basic_filebuf<CharT,Traits>`)

**C documentation for `fclose`**

---
*Source: https://en.cppreference.com/w/cpp/io/c/fclose*
