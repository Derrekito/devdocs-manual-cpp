# std::at_quick_exit

```cpp
int at_quick_exit( /*atexit-handler*/* func ) noexcept;
int at_quick_exit( /*c-atexit-handler*/* func ) noexcept;  // (1) (since C++11)
extern "C++" using /*atexit-handler*/ = void();
extern "C" using /*c-atexit-handler*/ = void();  // (2) (exposition only*)
```

Registers the function pointed to by `func` to be called on quick program
termination (via `std::quick_exit`).

Calling the function from several threads does not induce a data race. The
implementation is guaranteed to support the registration of at least 32
functions. The exact limit is implementation-defined.

The registered functions will not be called on normal program termination. If a
function need to be called in that case, `std::atexit` must be used.

### Parameters

- **func** — pointer to a function to be called on quick program termination

### Return value

`​0​` if the registration succeeds, nonzero value otherwise.

### Notes

The two overloads are distinct because the types of the parameter `func` are
distinct (language linkage is part of its type).

### Example

```cpp
#include <cstdlib>
#include <iostream>

void f1()
{
    std::cout << "pushed first" << std::endl; // flush is intentional
}

extern "C" void f2()
{
    std::cout << "pushed second\n";
}

int main()
{
    auto f3 = []
    {
        std::cout << "pushed third\n";
    };

    std::at_quick_exit(f1);
    std::at_quick_exit(f2);
    std::at_quick_exit(f3);
    std::quick_exit(0);
}
```

Output:

```text
pushed third
pushed second
pushed first
```

### See also

- **abort** — causes abnormal program termination (without cleaning up)
  (function)
- **exit** — causes normal program termination with cleaning up (function)
- **atexit** — registers a function to be called on `std::exit()` invocation
  (function)
- **quick_exit (C++11)** — causes quick program termination without completely
  cleaning up (function)

**C documentation for `at_quick_exit`**

---
*Source: https://en.cppreference.com/w/cpp/utility/program/at_quick_exit*
