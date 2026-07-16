# std::quick_exit

```cpp
[[noreturn]] void quick_exit( int exit_code ) noexcept;  // (since C++11)
```

Causes normal program termination to occur without completely cleaning the
resources.

Functions passed to `std::at_quick_exit` are called in reverse order of their
registration. If an exception tries to propagate out of any of the functions,
`std::terminate` is called. After calling the registered functions, calls
`std::_Exit(exit_code)`.

Functions passed to `std::atexit` are not called.

### Parameters

- **exit_code** — exit status of the program

### Return value

(none)

### Example

```cpp
#include <cstdlib>
#include <iostream>

template<int N>
void quick_exit_handler()
{
    std::cout << "quick_exit handler #" << N << std::endl; // flush is intended
}

void at_exit_handler()
{
    std::cout << "at_exit handler\n";
}

int main()
{
    if (std::at_quick_exit(quick_exit_handler<1>) ||
        std::at_quick_exit(quick_exit_handler<2>))
    {
        std::cerr << "Registration failed\n";
        return EXIT_FAILURE;
    }

    std::atexit(at_exit_handler); // the handler will not be called

    struct R { ~R() { std::cout << "destructor\n"; } } resource;

    /*...*/

    std::quick_exit(EXIT_SUCCESS);

    std::cout << "This statement is unreachable...\n";
}
```

Output:

```text
quick_exit handler #2
quick_exit handler #1
```

### See also

- **abort** — causes abnormal program termination (without cleaning up)
  (function)
- **exit** — causes normal program termination with cleaning up (function)
- **atexit** — registers a function to be called on `std::exit()` invocation
  (function)
- **at_quick_exit (C++11)** — registers a function to be called on
  `std::quick_exit` invocation (function)

**C documentation for `quick_exit`**

---
*Source: https://en.cppreference.com/w/cpp/utility/program/quick_exit*
