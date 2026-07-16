# std::future_error

```cpp
class future_error;  // (since C++11)
```

The class `std::future_error` defines an exception object that is thrown on
failure by the functions in the thread library that deal with asynchronous
execution and shared states (`std::future`, `std::promise`, etc). Similar to
`std::system_error`, this exception carries an error code compatible with
`std::error_code`.

### Member functions

- **(constructor)** — creates a `std::future_error` object (public member
  function)
- **operator=** — replaces the `std::future_error` object (public member
  function)
- **code** — returns the error code (public member function)
- **what** — returns the explanatory string specific to the error code (public
  member function)

## Inherited from std::logic_error

## Inherited from std::exception

### Member functions

- **(destructor) [virtual]** — destroys the exception object (virtual public
  member function of `std::exception`)
- **what [virtual]** — returns an explanatory string (virtual public member
  function of `std::exception`)

### Example

```cpp
#include <future>
#include <iostream>

int main()
{
    std::future<int> empty;
    try
    {
        int n = empty.get(); // The behavior is undefined, but
                             // some implementations throw std::future_error
    }
    catch (const std::future_error& e)
    {
        std::cout << "Caught a future_error with code \"" << e.code()
                  << "\"\nMessage: \"" << e.what() << "\"\n";
    }
}
```

Possible output:

```text
Caught a future_error with code "future:3"
Message: "No associated state"
```

### See also

- **future_errc (C++11)** — identifies the future error codes (enum)

---
*Source: https://en.cppreference.com/w/cpp/thread/future_error*
