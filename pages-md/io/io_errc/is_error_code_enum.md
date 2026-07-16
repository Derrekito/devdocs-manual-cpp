# std::is_error_code_enum<std::io_errc>

```cpp
template<>
struct is_error_code_enum<std::io_errc> : public std::true_type {};  // (since C++11)
```

This specialization of `std::is_error_code_enum` informs other library
components that values of type `std::io_errc` are enumerations that hold error
codes, which makes them implicitly convertible and assignable to objects of type
`std::error_code`.

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Example

The comparison between `e.code()` and `std::io_errc::stream` compiles because
`std::is_error_code_enum<std::io_errc>::value == true`.

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

- **is_error_code_enum (C++11)** — identifies a class as an `error_code`
  enumeration (class template)
- **error_code (C++11)** — holds a platform-dependent error code (class)
- **io_errc (C++11)** — the IO stream error codes (enum)

---
*Source: https://en.cppreference.com/w/cpp/io/io_errc/is_error_code_enum*
