# std::unexpected_handler

```cpp
typedef void (*unexpected_handler)();  // (deprecated in C++11) (removed in C++17)
```

`std::unexpected_handler` is the function pointer type (pointer to function that
takes no arguments and returns void), which is installed and queried by the
functions `std::set_unexpected` and `std::get_unexpected` and called by
`std::unexpected`.

The C++ implementation provides a default `std::unexpected_handler` function,
which calls `std::terminate()`. If the null pointer value is installed (by means
of `std::set_unexpected`), the implementation may restore the default handler
instead.

A user-defined `std::unexpected_handler` is expected to either terminate the
program or throw an exception. If it throws an exception, one of the following
three situations may be encountered:

1) the exception thrown by `std::unexpected_handler` satisfies the dynamic
exception specification that was violated earlier. The new exception is allowed
to escape the function and stack unwinding continues.

2) the exception thrown by `std::unexpected_handler` still violates the
exception specification:

2a) however, the exception specification allows `std::bad_exception`: the thrown
exception object is destroyed, and `std::bad_exception` is constructed by the
C++ runtime and thrown instead.

2b) the exception specification does not allow `std::bad_exception`:
`std::terminate()` is called.

### See also

- **unexpected (removed in C++17)** — function called when dynamic exception
  specification is violated (function)
- **set_unexpected (removed in C++17)** — changes the function to be called by
  `std::unexpected` (function)
- **get_unexpected (C++11)(removed in C++17)** — obtains the current
  unexpected_handler (function)

---
*Source: https://en.cppreference.com/w/cpp/error/unexpected_handler*
