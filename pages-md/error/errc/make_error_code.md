# std::make_error_code(std::errc)

```cpp
std::error_code make_error_code( std::errc e ) noexcept;  // (since C++11)
```

Creates error code value for `errc` enum `e`.

Equivalent to `std::error_code(static_cast<int>(e), std::generic_category())`

### Parameters

- **e** — error code enum to create error code for

### Return value

Error code corresponding to `e`.

### See also

- **make_error_code(std::io_errc) (C++11)** — constructs an iostream error code
  (function)
- **make_error_code(std::future_errc) (C++11)** — constructs a future error code
  (function)

---
*Source: https://en.cppreference.com/w/cpp/error/errc/make_error_code*
