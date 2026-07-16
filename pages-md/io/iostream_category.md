# std::iostream_category

```cpp
const std::error_category& iostream_category() noexcept;  // (since C++11)
```

Obtains a reference to the static error category object for iostream errors. The
object is required to override the virtual function `error_category::name()` to
return a pointer to the string `"iostream"`. It is used to identify error codes
provided in the exceptions of type `std::ios_base::failure`.

### Parameters

(none)

### Return value

A reference to the static object of unspecified runtime type, derived from
`std::error_category`.

### Example

```cpp
#include <fstream>
#include <iostream>

int main()
{
    std::ifstream f("doesn't exist");
    try
    {
        f.exceptions(f.failbit);
    }
    catch (const std::ios_base::failure& e)
    {
        std::cout << "Caught an ios_base::failure.\n"
                  << "Error code: " << e.code().value()
                  << " (" << e.code().message() << ")\n"
                  << "Error category: " << e.code().category().name() << '\n';

    }
}
```

Possible output:

```text
Caught an ios_base::failure.
Error code: 1 (unspecified iostream_category error)
Error category: iostream
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2087 | C++11 | `iostream_category` was not declared noexcept | declared
      noexcept

### See also

- **failure** — stream exception (public member class of `std::ios_base`)
- **io_errc (C++11)** — the IO stream error codes (enum)

---
*Source: https://en.cppreference.com/w/cpp/io/iostream_category*
