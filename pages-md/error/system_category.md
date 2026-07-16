# std::system_category

```cpp
const std::error_category& system_category() noexcept;  // (since C++11)
```

Obtains a reference to the static error category object for errors reported by
the operating system. The object is required to override the virtual function
`std::error_category::name()` to return a pointer to the string `"system"`. It
is also required to override the virtual function
`std::error_category::default_error_condition()` to map the error codes that
match POSIX `errno` values to `std::generic_category`.

### Parameters

(none)

### Return value

A reference to the static object of unspecified runtime type, derived from
`std::error_category`.

### Notes

On Windows, `system_category()` typically maps some Windows error codes to POSIX
ones. On POSIX, `system_category()` tends to be equivalent to
`std::generic_category()` except for the name.

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <string>
#include <system_error>

int main()
{
    for (int const code : {EDOM, 10001})
    {
        const std::error_condition econd =
            std::system_category().default_error_condition(code);

        std::cout << "Category: " << econd.category().name() << '\n'
                  << "Value:    " << econd.value() << '\n'
                  << "Message:  " << econd.message() << "\n\n";
    }
}
```

Possible output:

```text
Category: generic
Value:    33
Message:  Numerical argument out of domain

Category: system
Value:    10001
Message:  Unknown error 10001
```

### See also

- **generic_category (C++11)** — identifies the generic error category
  (function)
- **errc (C++11)** — the `std::error_condition` enumeration listing all standard
  `<cerrno>` macro constants (class)

---
*Source: https://en.cppreference.com/w/cpp/error/system_category*
