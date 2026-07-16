# std::io_errc

```cpp
enum class io_errc {
    stream = 1,
};  // (since C++11)
```

The scoped enumeration `std::io_errc` defines the error codes reported by I/O
streams in `std::ios_base::failure` exception objects. Only one error code
(`std::io_errc::stream`) is required, although the implementation may define
additional error codes. Because the appropriate specialization of
`std::is_error_code_enum` is provided, values of type `std::io_errc` are
implicitly convertible to `std::error_code`.

### Member constants

- **`stream`** — `1`

### Non-member functions

- **make_error_code(std::io_errc) (C++11)** — constructs an iostream error code
  (function)
- **make_error_condition(std::io_errc) (C++11)** — constructs an iostream error
  condition (function)

### Helper classes

- **is_error_code_enum<std::io_errc> (C++11)** — extends the type trait
  `std::is_error_code_enum` to identify iostream error codes (class template
  specialization)

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
        std::cout << "Caught an ios_base::failure.\n";
        if (e.code() == std::io_errc::stream)
            std::cout << "The error code is std::io_errc::stream\n";
    }
}
```

Output:

```text
Caught an ios_base::failure.
The error code is std::io_errc::stream
```

### See also

- **error_code (C++11)** — holds a platform-dependent error code (class)
- **error_condition (C++11)** — holds a portable error code (class)
- **failure** — stream exception (public member class of `std::ios_base`)

---
*Source: https://en.cppreference.com/w/cpp/io/io_errc*
