# std::puts

```cpp
int puts( const char *str );
```

Writes every character from the null-terminated string `str` and one additional
newline character `'\n'` to the output stream `stdout`, as if by repeatedly
executing `std::fputc`.

The terminating null character from `str` is not written.

### Parameters

- **str** — character string to be written

### Return value

On success, returns a non-negative value

On failure, returns `EOF` and sets the *error* indicator (see `std::ferror`) on
`stdout`.

### Notes

The `std::puts` function appends the newline character to the output, while
`std::fputs` function does not.

Different implementations return different non-negative numbers: some return the
last character written, some return the number of characters written (or
`INT_MAX` if the string was longer than that), some simply return a non-negative
constant.

A typical cause of failure for `std::puts` is running out of space on the file
system, when `stdout` is redirected to a file.

### Example

```cpp
#include <cstdio>

int main()
{
    int rc = std::puts("Hello World");

    if (rc == EOF)
        std::perror("puts()"); // POSIX requires that errno is set
}
```

Output:

```text
Hello World
```

### See also

- **fputs** — writes a character string to a file stream (function)
- **printffprintfsprintfsnprintf (C++11)** — prints formatted output to
  `stdout`, a file stream or a buffer (function)

**C documentation for `puts`**

---
*Source: https://en.cppreference.com/w/cpp/io/c/puts*
