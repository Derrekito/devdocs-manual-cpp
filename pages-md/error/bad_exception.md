# std::bad_exception

```cpp
class bad_exception;
```

`std::bad_exception` is the type of the exception thrown by the C++ runtime in
the following situations:

- If `std::exception_ptr` stores a copy of the caught exception and if the copy
  constructor of the exception object caught by `std::current_exception` throws
  an exception, the captured exception is an instance of `std::bad_exception`.
*(since C++11)*

- If a dynamic exception specification is violated and `std::unexpected` throws
  or rethrows an exception that still violates the exception specification, but
  the exception specification allows `std::bad_exception`, `std::bad_exception`
  is thrown.
*(until C++17)*

### Member functions

- **(constructor)** — constructs the `bad_exception` object (public member
  function)
- **operator=** — copies the object (public member function)
- **what [virtual]** — returns the explanatory string (virtual public member
  function)

## Inherited from std::exception

### Member functions

- **(destructor) [virtual]** — destroys the exception object (virtual public
  member function of `std::exception`)
- **what [virtual]** — returns an explanatory string (virtual public member
  function of `std::exception`)

### Example

Compiles only in C++14 (or earlier) mode.

```cpp
#include <exception>
#include <iostream>
#include <stdexcept>

void my_unexp()
{
    throw;
}

void test()
    throw(std::bad_exception) // Dynamic exception specifications
                              // are deprecated in C++11
{
    throw std::runtime_error("test");
}

int main()
{
    std::set_unexpected(my_unexp); // Deprecated in C++11, removed in C++17

    try
    {
        test();
    }
    catch (const std::bad_exception& e)
    {
        std::cerr << "Caught " << e.what() << '\n';
    }
}
```

Possible output:

```text
Caught std::bad_exception
```

---
*Source: https://en.cppreference.com/w/cpp/error/bad_exception*
