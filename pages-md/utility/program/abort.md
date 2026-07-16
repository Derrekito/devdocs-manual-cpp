# std::abort

```cpp
void abort();  // (until C++11)
[[noreturn]] void abort() noexcept;  // (since C++11)
```

Causes abnormal program termination unless `SIGABRT` is being caught by a signal
handler passed to `std::signal` and the handler does not return.

Destructors of variables with automatic, thread local(since C++11) and static
storage durations are not called. Functions registered with `std::atexit()` and
`std::at_quick_exit`(since C++11) are also not called. Whether open resources
such as files are closed is implementation defined. An implementation defined
status is returned to the host environment that indicates unsuccessful
execution.

### Parameters

(none)

### Return value

None because it does not return.

### Exceptions

Throws nothing.

### Notes

POSIX specifies that the `abort()` function overrides blocking or ignoring the
`SIGABRT` signal.

Some compiler intrinsics, e.g. `__builtin_trap` (gcc, clang, and icc) or
`__fastfail`/`__debugbreak` (msvc), can be used to terminate the program as fast
as possible.

### Example

```cpp
#include <csignal>
#include <cstdlib>
#include <iostream>

class Tester
{
public:
    Tester()  { std::cout << "Tester ctor\n"; }
    ~Tester() { std::cout << "Tester dtor\n"; }
};

Tester static_tester; // Destructor not called

void signal_handler(int signal)
{
    if (signal == SIGABRT)
        std::cerr << "SIGABRT received\n";
    else
        std::cerr << "Unexpected signal " << signal << " received\n";
    std::_Exit(EXIT_FAILURE);
}

int main()
{
    Tester automatic_tester; // Destructor not called

    // Setup handler
    auto previous_handler = std::signal(SIGABRT, signal_handler);
    if (previous_handler == SIG_ERR)
    {
        std::cerr << "Setup failed\n";
        return EXIT_FAILURE;
    }

    std::abort(); // Raise SIGABRT
    std::cout << "This code is unreachable\n";
}
```

Output:

```text
Tester ctor
Tester ctor
SIGABRT received
```

### See also

- **exit** — causes normal program termination with cleaning up (function)
- **atexit** — registers a function to be called on `std::exit()` invocation
  (function)
- **quick_exit (C++11)** — causes quick program termination without completely
  cleaning up (function)
- **at_quick_exit (C++11)** — registers a function to be called on
  `std::quick_exit` invocation (function)
- **signal** — sets a signal handler for particular signal (function)
- **terminate** — function called when exception handling fails (function)

**C documentation for `abort`**

---
*Source: https://en.cppreference.com/w/cpp/utility/program/abort*
