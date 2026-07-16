# std::strerror

```cpp
char* strerror( int errnum );
```

Returns a pointer to the textual description of the system error code `errnum`,
identical to the description that would be printed by `std::perror()`.

`errnum` is usually acquired from the `errno` variable, however the function
accepts any value of type int. The contents of the string are locale-specific.

The returned string must not be modified by the program, but may be overwritten
by a subsequent call to the `strerror` function. `strerror` is not required to
be thread-safe. Implementations may be returning different pointers to static
read-only string literals or may be returning the same pointer over and over,
pointing at a static buffer in which `strerror` places the string.

### Parameters

- **errnum** — integer value referring to an error code

### Return value

Pointer to a null-terminated byte string corresponding to the `errno` error code
`errnum`.

### Notes

POSIX allows subsequent calls to `strerror` to invalidate the pointer value
returned by an earlier call. It also specifies that it is the `LC_MESSAGES`
locale facet that controls the contents of these messages.

POSIX has a thread-safe version called `strerror_r` defined. Glibc defines an
incompatible version.

### Example

```cpp
#include <cerrno>
#include <clocale>
#include <cmath>
#include <cstring>
#include <iostream>

int main()
{
    const double not_a_number = std::log(-1.0);
    std::cout << not_a_number << '\n';

    if (errno == EDOM)
    {
        std::cout << "log(-1) failed: " << std::strerror(errno) << '\n';
        std::setlocale(LC_MESSAGES, "de_DE.utf8");
        std::cout << "Or, in German, " << std::strerror(errno) << '\n';
    }
}
```

Possible output:

```text
nan
log(-1) failed: Numerical argument out of domain
Or, in German, Das numerische Argument ist ausserhalb des Definitionsbereiches
```

### See also

- **perror** — displays a character string corresponding of the current error to
  `stderr` (function)
- **E2BIG, EACCES, ..., EXDEV** — macros for standard POSIX-compatible error
  conditions (macro constant)

**C documentation for `strerror`**

---
*Source: https://en.cppreference.com/w/cpp/string/byte/strerror*
