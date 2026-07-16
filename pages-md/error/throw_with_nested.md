# std::throw_with_nested

```cpp
template< class T >
[[noreturn]] void throw_with_nested( T&& t );  // (since C++11)
```

If `std::decay<T>::type` is a non-final non-union class type that is neither
`std::nested_exception` nor derived from `std::nested_exception`, throws an
exception of an unspecified type that is publicly derived from both
`std::nested_exception` and from `std::decay<T>::type`, and constructed from
`std::forward<T>(t)`. The default constructor of the `nested_exception` base
class calls `std::current_exception`, capturing the currently handled exception
object, if any, in a `std::exception_ptr`.

Otherwise, throws `std::forward<T>(t)`.

Requires that `std::decay<T>::type` is CopyConstructible.

### Parameters

- **t** — the exception object to throw

### Return value

(none)

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

- **nested_exception (C++11)** — a mixin type to capture and store current
  exceptions (class)
- **rethrow_if_nested (C++11)** — throws the exception from a
  `std::nested_exception` (function template)

---
*Source: https://en.cppreference.com/w/cpp/error/throw_with_nested*
