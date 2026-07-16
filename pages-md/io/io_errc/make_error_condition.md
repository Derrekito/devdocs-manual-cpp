# std::make_error_condition(std::io_errc)

```cpp
std::error_condition make_error_condition( std::io_errc e ) noexcept;  // (since C++11)
```

Constructs an `std::error_condition` object from a value of type `std::io_errc`
as if by `return std::error_condition(static_cast<int>(e),
std::iostream_category())`.

### Parameters

- **e** — error code number

### Return value

A value of type `std::error_condition` that holds the error code number from `e`
associated with the error category `"iostream"`.

### Example

```cpp
#include <iostream>
#include <system_error>

int main()
{
    std::error_condition ec = std::make_error_condition(std::io_errc::stream);
    std::cout << "error condition for io_errc::stream has value " << ec.value()
              << "\nand message \"" << ec.message() << "\"\n";
}
```

Output:

```text
error condition for io_errc::stream has value 1
and message "unspecified iostream_category error"
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2087 | C++11 | `make_error_condition(io_errc)` was not declared noexcept |
      declared noexcept

### See also

- **error_condition (C++11)** — holds a portable error code (class)
- **io_errc (C++11)** — the IO stream error codes (enum)

---
*Source: https://en.cppreference.com/w/cpp/io/io_errc/make_error_condition*
