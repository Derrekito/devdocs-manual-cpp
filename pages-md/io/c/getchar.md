# std::getchar

```cpp
int getchar();
```

Reads the next character from `stdin`.

Equivalent to `std::getc(stdin)`.

### Parameters

(none)

### Return value

The obtained character on success or `EOF` on failure.

If the failure has been caused by end of file condition, additionally sets the
*eof* indicator (see `std::feof()`) on `stdin`. If the failure has been caused
by some other error, sets the *error* indicator (see `std::ferror()`) on
`stdin`.

### Example

`std::getchar` with error checking. Exit program by entering ESC char.

```cpp
#include <cctype>
#include <cstdio>
#include <cstdlib>
#include <iomanip>
#include <iostream>

int main()
{
    for (int ch; (ch = std::getchar()) != EOF ;) // read/print "abc" from stdin
    {
        if (std::isprint(ch))
            std::cout << static_cast<char>(ch) << '\n';
        if (ch == 27) // 'ESC' (escape) in ASCII
            return EXIT_SUCCESS;
    }

    // Test reason for reaching EOF.
    if (std::feof(stdin)) // if failure caused by end-of-file condition
        std::cout << "End of file reached\n";
    else if (std::ferror(stdin)) // if failure caused by some other error
    {
        std::perror("getchar()");
        std::cerr << "getchar() failed in file " << std::quoted(__FILE__)
                  << " at line # " << __LINE__ - 14 << '\n';
        std::exit(EXIT_FAILURE);
    }

    return EXIT_SUCCESS;
}
```

Possible output:

```cpp
abc
a
b
c
^[
```

### See also

- **fgetcgetc** — gets a character from a file stream (function)

**C documentation for `getchar`**

---
*Source: https://en.cppreference.com/w/cpp/io/c/getchar*
