# std::regex_error

```cpp
class regex_error;  // (since C++11)
```

Defines the type of exception object thrown to report errors in the regular
expressions library.

### Member functions

- **(constructor)** — constructs a `regex_error` object (public member function)
- **operator=** — replaces the `regex_error` object (public member function)
- **code** — gets the `std::regex_constants::error_type` for a `regex_error`
  (public member function)

## Inherited from std::runtime_error

## Inherited from std::exception

### Member functions

- **(destructor) [virtual]** — destroys the exception object (virtual public
  member function of `std::exception`)
- **what [virtual]** — returns an explanatory string (virtual public member
  function of `std::exception`)

### Example

```cpp
#include <iostream>
#include <regex>

int main()
{
    try
    {
        std::regex re("[a-b][a");
    }
    catch (const std::regex_error& e)
    {
        std::cout << "regex_error caught: " << e.what() << '\n';
        if (e.code() == std::regex_constants::error_brack)
            std::cout << "The code was error_brack\n";
    }
}
```

Possible output:

```text
regex_error caught: The expression contained mismatched [ and ].
The code was error_brack
```

---
*Source: https://en.cppreference.com/w/cpp/regex/regex_error*
