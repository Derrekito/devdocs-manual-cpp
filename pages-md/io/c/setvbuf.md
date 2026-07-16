# std::setvbuf

```cpp
int setvbuf( std::FILE* stream, char* buffer, int mode, std::size_t size );
```

Changes the buffering mode of the given file stream `stream` as indicated by the
argument `mode`. In addition,

- If `buffer` is a null pointer, resizes the internal buffer to `size`.
- If `buffer` is not a null pointer, instructs the stream to use the
  user-provided buffer of size `size` beginning at `buffer`. The stream must be
  closed (with `std::fclose`) before the lifetime of the array pointed to by
  `buffer` ends. The contents of the array after a successful call to
  `std::setvbuf` are indeterminate and any attempt to use it is undefined
  behavior.

### Parameters

- **stream** — the file stream to set the buffer to
- **buffer** — pointer to a buffer for the stream to use or null pointer to
  change size and mode only
- **mode** — buffering mode to use. It can be one of the following values:
  _IOFBF full buffering _IOLBF line buffering _IONBF no buffering
- **_IOFBF** — full buffering
- **_IOLBF** — line buffering
- **_IONBF** — no buffering
- **size** — size of the buffer

### Return value

`​0​` on success or nonzero on failure.

### Notes

This function may only be used after `stream` has been associated with an open
file, but before any other operation (other than a failed call to
`std::setbuf`/`std::setvbuf`).

Not all `size` bytes will necessarily be used for buffering: the actual buffer
size is usually rounded down to a multiple of 2, a multiple of page size, etc.

On many implementations, line buffering is only available for terminal input
streams.

A common error is setting the buffer of `stdin` or `stdout` to an array whose
lifetime ends before the program terminates:

```cpp
int main()
{
    char buf[BUFSIZ];
    std::setbuf(stdin, buf);
} // lifetime of buf ends, undefined behavior
```

The default buffer size `BUFSIZ` is expected to be the most efficient buffer
size for file I/O on the implementation, but POSIX `fstat` often provides a
better estimate.

### Example

One use case for changing buffer size is when a better size is known.

```cpp
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <sys/stat.h>

int main()
{
    std::FILE* fp = std::fopen("/tmp/test.txt", "w+");
    if (!fp)
    {
        std::perror("fopen");
        return EXIT_FAILURE;
    }

    struct stat stats;
    if (fstat(fileno(fp), &stats) == -1) // POSIX only
    {
        std::perror("fstat");
        return EXIT_FAILURE;
    }

    std::cout << "BUFSIZ is " << BUFSIZ << ", but optimal block size is "
              << stats.st_blksize << '\n';
    if (std::setvbuf(fp, nullptr, _IOFBF, stats.st_blksize) != 0)
    {
        std::perror("setvbuf failed"); // POSIX version sets errno
        return EXIT_FAILURE;
    }

    // Read entire file: use truss/strace to observe the read(2) syscalls used
    for (int ch; (ch = std::fgetc(fp)) != EOF;)
    {}

    std::fclose(fp);
    return EXIT_SUCCESS;
}
```

Possible output:

```text
BUFSIZ is 8192, but optimal block size is 65536
```

### See also

- **setbuf** — sets the buffer for a file stream (function)
- **setbuf [virtual]** — provides user-supplied buffer or turns this filebuf
  unbuffered (virtual protected member function of
  `std::basic_filebuf<CharT,Traits>`)

**C documentation for `setvbuf`**

---
*Source: https://en.cppreference.com/w/cpp/io/c/setvbuf*
