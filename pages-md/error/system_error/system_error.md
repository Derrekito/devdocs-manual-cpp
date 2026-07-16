# std::system_error::system_error

```cpp
system_error( std::error_code ec );  // (1) (since C++11)
system_error( std::error_code ec, const std::string& what_arg );  // (2) (since C++11)
system_error( std::error_code ec, const char* what_arg );  // (2) (since C++11)
system_error( int ev, const std::error_category& ecat );  // (3) (since C++11)
system_error( int ev, const std::error_category& ecat,
              const std::string& what_arg );  // (4) (since C++11)
system_error( int ev, const std::error_category& ecat,
              const char* what_arg );  // (4) (since C++11)
system_error( const system_error& other ) noexcept;  // (5) (since C++11)
```

Constructs new system error object.

1) Constructs with error code `ec`.

2) Constructs with error code `ec` and explanation string `what_arg`. The string
   returned by `what()` is guaranteed to contain `what_arg` as a substring.

3) Constructs with underlying error code `ev` and associated error category
   `ecat`.

4) Constructs with underlying error code `ev`, associated error category `ecat`
   and explanatory string `what_arg`. The string returned by `what()` is
   guaranteed to contain `what_arg` as a substring (assuming that it doesn't
   contain an embedded null character).

5) Copy constructor. Initializes the contents with those of `other`. If `*this`
   and `other` both have dynamic type `std::system_error` then
   `std::strcmp(what(), other.what()) == 0`.

### Parameters

- **ec** — error code
- **ev** — underlying error code in the enumeration associated with `ecat`
- **ecat** — the category of error
- **what_arg** — explanatory string
- **other** — another `system_error` to copy

### Example

Demonstrates how to create a `system_error` exception from an `errno` value.

```cpp
#include <iostream>
#include <system_error>

int main()
{
    try
    {
        throw std::system_error(EDOM, std::generic_category(), "FIX ME");
    }
    catch (const std::system_error& ex)
    {
        std::cout << "code:    [" << ex.code() << "]\n"
                     "message: [" << ex.code().message() << "]\n"
                     "what:    [" << ex.what() << "]\n";
    }
}
```

Possible output:

```text
code:    [generic:33]
message: [Numerical argument out of domain]
what:    [FIX ME: Numerical argument out of domain]
```

---
*Source: https://en.cppreference.com/w/cpp/error/system_error/system_error*
