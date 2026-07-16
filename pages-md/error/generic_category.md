# std::generic_category

```cpp
const std::error_category& generic_category() noexcept;  // (since C++11)
```

Obtains a reference to the static error category object for generic errors. The
object is required to override the virtual function `error_category::name()` to
return a pointer to the string `"generic"`. It is used to identify error
conditions that correspond to the POSIX `errno` codes.

### Parameters

(none)

### Return value

A reference to the static object of unspecified runtime type, derived from
`std::error_category`.

### Example

```cpp
#include <cerrno>
#include <iostream>
#include <string>
#include <system_error>

int main()
{
    std::error_condition econd = std::generic_category().default_error_condition(EDOM);
    std::cout << "Category: " << econd.category().name() << '\n'
              << "Value: " << econd.value() << '\n'
              << "Message: " << econd.message() << '\n';
}
```

Output:

```text
Category: generic
Value: 33
Message: Numerical argument out of domain
```

### See also

- **system_category (C++11)** — identifies the operating system error category
  (function)
- **errc (C++11)** — the `std::error_condition` enumeration listing all standard
  `<cerrno>` macro constants (class)

---
*Source: https://en.cppreference.com/w/cpp/error/generic_category*
