# std::new_handler

```cpp
typedef void (*new_handler)();
```

`std::new_handler` is the function pointer type (pointer to function that takes
no arguments and returns void), which is used by the functions
`std::set_new_handler` and `std::get_new_handler`.

### See also

- **operator newoperator new[]** — allocation functions (function)
- **set_new_handler** — registers a new handler (function)
- **get_new_handler (C++11)** — obtains the current new handler (function)

---
*Source: https://en.cppreference.com/w/cpp/memory/new/new_handler*
