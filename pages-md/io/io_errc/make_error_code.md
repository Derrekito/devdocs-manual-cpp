# std::make_error_code(std::io_errc)

```cpp
std::error_code make_error_code( std::io_errc e ) noexcept;  // (since C++11)
```

Constructs an `std::error_code` object from a value of type `std::io_errc` as if
by `return std::error_code(static_cast<int>(e), std::iostream_category())`.

This function is called by the constructor of `std::error_code` with an
`std::io_errc` argument.

### Parameters

- **e** — error code number

### Return value

A value of type `std::error_code` that holds the error code number from `e`
associated with the error category `"iostream"`.

### Example

```cpp
#include <iostream>
#include <system_error>

int main()
{
    std::error_code ec = std::make_error_code(std::io_errc::stream);

    // This works because of the overloaded method
    //    and the is_error_code_enum specialization.
    ec = std::io_errc::stream;

    std::cout << "Error code from io_errc::stream has category "
              << ec.category().name() << '\n';
}
```

Output:

```text
Error code from io_errc::stream has category iostream
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2087 | C++11 | `make_error_code(io_errc)` was not declared noexcept |
      declared noexcept

### See also

- **error_code (C++11)** — holds a platform-dependent error code (class)
- **io_errc (C++11)** — the IO stream error codes (enum)
- **make_error_code(std::errc) (C++11)** — creates error code value for `errc`
  enum `e` (function)
- **make_error_code(std::future_errc) (C++11)** — constructs a future error code
  (function)

---
*Source: https://en.cppreference.com/w/cpp/io/io_errc/make_error_code*
