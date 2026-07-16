# std::set_unexpected

```cpp
std::unexpected_handler set_unexpected( std::unexpected_handler f ) throw();  // (until C++11)
std::unexpected_handler set_unexpected( std::unexpected_handler f ) noexcept;  // (since C++11) (deprecated) (removed in C++17)
```

Makes `f` the new global `std::unexpected_handler` and returns the previously
installed `std::unexpected_handler`.

This function is thread-safe. Every call to `std::set_unexpected`
*synchronizes-with* (see `std::memory_order`) the subsequent calls to
`std::set_unexpected` and `std::get_unexpected`
*(since C++11)*

### Parameters

- **f** — pointer to function of type `std::unexpected_handler`, or null pointer

### Return value

The previously-installed unexpected handler, or a null pointer value if none was
installed.

### See also

- **unexpected (removed in C++17)** — function called when dynamic exception
  specification is violated (function)
- **get_unexpected (C++11)(removed in C++17)** — obtains the current
  unexpected_handler (function)
- **unexpected_handler (removed in C++17)** — the type of the function called by
  `std::unexpected` (typedef)

---
*Source: https://en.cppreference.com/w/cpp/error/set_unexpected*
