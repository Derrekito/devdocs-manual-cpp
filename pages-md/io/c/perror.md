# std::perror

```cpp
void perror( const char *s );
```

Prints a textual description of the error code currently stored in the system
variable `errno` to `stderr`.

The description is formed by concatenating the following components:

- the contents of the null-terminated byte string pointed to by `s`, followed by
  `": "` (unless `s` is a null pointer or the character pointed to by `s` is the
  null character).
- implementation-defined error message string describing the error code stored
  in `errno`, followed by `'\n'`. The error message string is identical to the
  result of `std::strerror(errno)`.

### Parameters

- **s** — pointer to a null-terminated string with explanatory message

### Return value

(none)

### Example

```cpp
#include <cerrno>
#include <cmath>
#include <cstdio>

int main()
{
    double not_a_number = std::log(-1.0);
    if (errno == EDOM)
        std::perror("log(-1) failed");
    std::printf("%f\n", not_a_number);
}
```

Possible output:

```text
log(-1) failed: Numerical argument out of domain
nan
```

### See also

- **errno** — macro which expands to POSIX-compatible thread-local error number
  variable (macro variable)
- **strerror** — returns a text version of a given error code (function)

**C documentation for `perror`**

---
*Source: https://en.cppreference.com/w/cpp/io/c/perror*
