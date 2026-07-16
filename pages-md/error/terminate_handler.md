# std::terminate_handler

```cpp
typedef void (*terminate_handler)();
```

`std::terminate_handler` is the function pointer type (pointer to function that
takes no arguments and returns void), which is installed and queried by the
functions `std::set_terminate` and `std::get_terminate` and called by
`std::terminate`.

A `std::terminate_handler` shall terminate execution of the program without
returning to the caller, otherwise the behavior is undefined.

The C++ implementation provides a default `std::terminate_handler` function,
which calls `std::abort()`. If the null pointer value is installed (by means of
`std::set_terminate`), the implementation may restore the default handler
instead.

### See also

- **terminate** — function called when exception handling fails (function)
- **set_terminate** — changes the function to be called by `std::terminate`
  (function)
- **get_terminate (C++11)** — obtains the current terminate_handler (function)

---
*Source: https://en.cppreference.com/w/cpp/error/terminate_handler*
