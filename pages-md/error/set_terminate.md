# std::set_terminate

```cpp
std::terminate_handler set_terminate( std::terminate_handler f ) throw();  // (until C++11)
std::terminate_handler set_terminate( std::terminate_handler f ) noexcept;  // (since C++11)
```

Makes `f` the new global terminate handler function and returns the previously
installed `std::terminate_handler`. `f` shall terminate execution of the program
without returning to its caller, otherwise the behavior is undefined.

This function is thread-safe. Every call to `std::set_terminate`
*synchronizes-with* (see `std::memory_order`) subsequent calls to
`std::set_terminate` and `std::get_terminate`.
*(since C++11)*

### Parameters

- **f** — pointer to function of type `std::terminate_handler`, or null pointer

### Return value

The previously-installed terminate handler, or a null pointer value if none was
installed.

### Example

```cpp
#include <cstdlib>
#include <exception>
#include <iostream>

int main()
{
    std::set_terminate([]()
    {
        std::cout << "Unhandled exception\n" << std::flush;
        std::abort();
    });
    throw 1;
}
```

Possible output:

```text
Unhandled exception
bash: line 7:  7743 Aborted                 (core dumped) ./a.out
```

### See also

- **terminate** — function called when exception handling fails (function)
- **get_terminate (C++11)** — obtains the current terminate_handler (function)
- **terminate_handler** — the type of the function called by `std::terminate`
  (typedef)

---
*Source: https://en.cppreference.com/w/cpp/error/set_terminate*
