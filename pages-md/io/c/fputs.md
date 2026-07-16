# std::fputs

```cpp
int fputs( const char* str, std::FILE* stream );
```

Writes every character from the null-terminated string `str` to the output
stream `stream`, as if by repeatedly executing `std::fputc`.

The terminating null character from `str` is not written.

### Parameters

- **str** — null-terminated character string to be written
- **stream** — output stream

### Return value

On success, returns a non-negative value

On failure, returns `EOF` and sets the *error* indicator (see `std::ferror`) on
`stream`.

### Notes

The related function `std::puts` appends a newline character to the output,
while `std::fputs` writes the string unmodified.

Different implementations return different non-negative numbers: some return the
last character written, some return the number of characters written (or
`INT_MAX` if the string was longer than that), some simply return a non-negative
constant such as zero.

### Example

```cpp
#include <cstdio>

int main(void)
{
    int rc = std::fputs("Hello World", stdout);

    if (rc == EOF)
        std::perror("fputs()"); // POSIX requires that errno is set
}
```

Output:

```text
Hello World
```

### See also

- **printffprintfsprintfsnprintf (C++11)** — prints formatted output to
  `stdout`, a file stream or a buffer (function)
- **puts** — writes a character string to `stdout` (function)
- **fputws** — writes a wide string to a file stream (function)
- **fgets** — gets a character string from a file stream (function)

**C documentation for `fputs`**

---
*Source: https://en.cppreference.com/w/cpp/io/c/fputs*
