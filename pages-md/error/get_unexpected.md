# std::get_unexpected

```cpp
std::unexpected_handler get_unexpected() noexcept;  // (since C++11) (deprecated) (removed in C++17)
```

Returns the currently installed `std::unexpected_handler`, which may be a null
pointer.

This function is thread-safe. Prior call to `std::set_unexpected`
*synchronizes-with* (see `std::memory_order`) the subsequent calls to this
function.

### Parameters

(none)

### Return value

The currently installed `std::unexpected_handler`.

### See also

- **unexpected_handler (removed in C++17)** — the type of the function called by
  `std::unexpected` (typedef)
- **set_unexpected (removed in C++17)** — changes the function to be called by
  `std::unexpected` (function)

---
*Source: https://en.cppreference.com/w/cpp/error/get_unexpected*
