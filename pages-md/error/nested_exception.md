# std::nested_exception

```cpp
class nested_exception;  // (since C++11)
```

`std::nested_exception` is a polymorphic mixin class which can capture and store
the current exception, making it possible to nest exceptions of arbitrary types
within each other.

### Member functions

- **(constructor)** — constructs a nested_exception (public member function)
- **(destructor) [virtual]** — destructs a nested exception (virtual public
  member function)
- **operator=** — replaces the contents of a nested_exception (public member
  function)
- **rethrow_nested** — throws the stored exception (public member function)
- **nested_ptr** — obtains a pointer to the stored exception (public member
  function)

### Non-member functions

- **throw_with_nested (C++11)** — throws its argument with
  `std::nested_exception` mixed in (function template)
- **rethrow_if_nested (C++11)** — throws the exception from a
  `std::nested_exception` (function template)

### Example

Demonstrates construction and recursion through a nested exception object.

```cpp
#include <exception>
#include <fstream>
#include <iostream>
#include <stdexcept>
#include <string>

// prints the explanatory string of an exception. If the exception is nested,
// recurses to print the explanatory of the exception it holds
void print_exception(const std::exception& e, int level =  0)
{
    std::cerr << std::string(level, ' ') << "exception: " << e.what() << '\n';
    try
    {
        std::rethrow_if_nested(e);
    }
    catch (const std::exception& nestedException)
    {
        print_exception(nestedException, level + 1);
    }
    catch (...) {}
}

// sample function that catches an exception and wraps it in a nested exception
void open_file(const std::string& s)
{
    try
    {
        std::ifstream file(s);
        file.exceptions(std::ios_base::failbit);
    }
    catch (...)
    {
        std::throw_with_nested(std::runtime_error("Couldn't open " + s));
    }
}

// sample function that catches an exception and wraps it in a nested exception
void run()
{
    try
    {
        open_file("nonexistent.file");
    }
    catch (...)
    {
        std::throw_with_nested(std::runtime_error("run() failed"));
    }
}

// runs the sample function above and prints the caught exception
int main()
{
    try
    {
        run();
    }
    catch (const std::exception& e)
    {
        print_exception(e);
    }
}
```

Possible output:

```text
exception: run() failed
 exception: Couldn't open nonexistent.file
  exception: basic_ios::clear
```

### See also

- **exception_ptr (C++11)** — shared pointer type for handling exception objects
  (typedef)
- **throw_with_nested (C++11)** — throws its argument with
  `std::nested_exception` mixed in (function template)
- **rethrow_if_nested (C++11)** — throws the exception from a
  `std::nested_exception` (function template)

---
*Source: https://en.cppreference.com/w/cpp/error/nested_exception*
