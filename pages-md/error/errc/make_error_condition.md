# std::make_error_condition(std::errc)

```cpp
std::error_condition make_error_condition( std::errc e ) noexcept;  // (since C++11)
```

Creates an error condition for an `errc` value `e`. Sets the error value to
`int(e)` and error category to `std::generic_category`.

### Parameters

- **e** — standard error value

### Return value

Error condition for `e`.

### Example

```cpp
#include <iostream>
#include <string>
#include <system_error>

int main()
{
    auto err = std::make_error_condition(std::errc::invalid_argument);
    std::cout << err.message() << '\n';
}
```

Possible output:

```text
Invalid argument
```

---
*Source: https://en.cppreference.com/w/cpp/error/errc/make_error_condition*
