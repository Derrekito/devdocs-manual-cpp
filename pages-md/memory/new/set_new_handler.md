# std::set_new_handler

```cpp
std::new_handler set_new_handler( std::new_handler new_p ) throw();  // (until C++11)
std::new_handler set_new_handler( std::new_handler new_p ) noexcept;  // (since C++11)
```

Makes `new_p` the new global new-handler function and returns the previously
installed new-handler.

The *new-handler* function is the function called by allocation functions
whenever a memory allocation attempt fails. Its intended purpose is one of three
things:

1) make more memory available,

2) terminate the program (e.g. by calling `std::terminate`),

3) throw exception of type `std::bad_alloc` or derived from `std::bad_alloc`.

The default implementation throws `std::bad_alloc`. The user can install their
own *new-handler*, which may offer behavior different than the default one.

If *new-handler* returns, the allocation function repeats the previously-failed
allocation attempt and calls the *new-handler* again if the allocation fails
again. To end the loop, *new-handler* may call `std::set_new_handler(nullptr)`:
if, after a failed allocation attempt, allocation function finds that
`std::get_new_handler` returns a null pointer value, it will throw
`std::bad_alloc`.

At program startup, *new-handler* is a null pointer.

This function is thread-safe. Every call to `std::set_new_handler`
*synchronizes-with* (see `std::memory_order`) the subsequent
`std::set_new_handler` and `std::get_new_handler` calls.
*(since C++11)*

### Parameters

- **new_p** — pointer to function of type `std::new_handler`, or null pointer

### Return value

The previously-installed new handler, or a null pointer value if none was
installed.

### Example

```cpp
#include <iostream>
#include <new>

void handler()
{
    std::cout << "Memory allocation failed, terminating\n";
    std::set_new_handler(nullptr);
}

int main()
{
    std::set_new_handler(handler);
    try
    {
        while (true)
        {
            new int[1000'000'000ul]();
        }
    }
    catch (const std::bad_alloc& e)
    {
        std::cout << e.what() << '\n';
    }
}
```

Possible output:

```text
Memory allocation failed, terminating
std::bad_alloc
```

### See also

- **operator newoperator new[]** — allocation functions (function)
- **get_new_handler (C++11)** — obtains the current new handler (function)
- **new_handler** — function pointer type of the new handler (typedef)
- **bad_alloc** — exception thrown when memory allocation fails (class)

---
*Source: https://en.cppreference.com/w/cpp/memory/new/set_new_handler*
