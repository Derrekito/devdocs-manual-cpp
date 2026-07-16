# std::freopen

```cpp
std::FILE* freopen( const char* filename, const char* mode, std::FILE* stream );
```

First, attempts to close the file associated with `stream`, ignoring any errors.
Then, if `filename` is not null, attempts to open the file specified by
`filename` using `mode` as if by `std::fopen`, and associates that file with the
file stream pointed to by `stream`. If `filename` is a null pointer, then the
function attempts to reopen the file that is already associated with `stream`
(it is implementation defined which mode changes are allowed in this case).

### Parameters

- **filename** — file name to associate the file stream to
- **mode** — null-terminated character string determining new file access mode
- **stream** — the file stream to modify

### File access flags

  File access mode string | Meaning | Explanation | Action if file already
      exists | Action if file does not exist
  `"r"` | read | Open a file for reading | read from start | return `NULL` and
      set error
  `"w"` | write | Create a file for writing | destroy contents | create new
  `"a"` | append | Append to a file | write to end | create new
  `"r+"` | read extended | Open a file for read/write | read from start | return
      `NULL` and set error
  `"w+"` | write extended | Create a file for read/write | destroy contents |
      create new
  `"a+"` | append extended | Open a file for read/write | write to end | create
      new
  File access mode flag `"b"` can optionally be specified to open a file in
      binary mode. This flag has no effect on POSIX systems, but on Windows, for
      example, it disables special handling of `'\n'` and `'\x1A'`. On the
      append file access modes, data is written to the end of the file
      regardless of the current position of the file position indicator.
  File access mode flag `"x"` can optionally be appended to `"w"` or `"w+"`
      specifiers. This flag forces the function to fail if the file exists,
      instead of overwriting it. (C++17)
  The behavior is undefined if the mode is not one of the strings listed above.
      Some implementations define additional supported modes (e.g. Windows).

### Return value

`stream` on success, a null pointer on failure.

### Notes

`std::freopen` is the only way to change the narrow/wide orientation of a stream
once it has been established by an I/O operation or by `std::fwide`.

Microsoft CRT version of `std::freopen` does not support any mode changes when
`filename` is a null pointer and treats this as an error (see documentation). A
possible workaround is the non-standard function `_setmode()`.

### Example

The following code redirects `stdout` to a file.

```cpp
#include <cstdio>

int main()
{
    std::printf("stdout is printed to console\n");
    if (std::freopen("redir.txt", "w", stdout))
    {
        std::printf("stdout is redirected to a file\n"); // this is written to redir.txt
        std::fclose(stdout);
    }
}
```

Output:

```text
stdout is printed to console
```

### See also

- **fopen** — opens a file (function)
- **fclose** — closes a file (function)

**C documentation for `freopen`**

---
*Source: https://en.cppreference.com/w/cpp/io/c/freopen*
