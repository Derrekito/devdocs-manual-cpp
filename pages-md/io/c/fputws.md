# std::fputws

```cpp
int fputws( const wchar_t* str, std::FILE* stream );
```

Writes every wide character from the null-terminated wide string `str` to the
output stream `stream`, as if by repeatedly executing `std::fputwc`.

The terminating null wide character from `str` is not written.

### Parameters

- **str** — null-terminated wide string to be written
- **stream** — output stream

### Return value

On success, returns a non-negative value

On failure, returns `EOF` and sets the *error* indicator (see `std::ferror`) on
`stream`.

### Example

```cpp
#include <clocale>
#include <cstdio>
#include <cwchar>

int main()
{
    std::setlocale(LC_ALL, "en_US.utf8");
    int rc = std::fputws(L"御休みなさい", stdout);

    if (rc == EOF)
        std::perror("fputws()"); // POSIX requires that errno is set
}
```

Possible output:

```text
御休みなさい
```

### See also

- **fputs** — writes a character string to a file stream (function)
- **wprintffwprintfswprintf** — prints formatted wide character output to
  `stdout`, a file stream or a buffer (function)
- ****fputws**** — writes a wide string to a file stream (function)
- **fgetws** — gets a wide string from a file stream (function)

**C documentation for `fputws`**

---
*Source: https://en.cppreference.com/w/cpp/io/c/fputws*
