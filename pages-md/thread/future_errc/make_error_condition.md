# std::make_error_condition(std::future_errc)

```cpp
std::error_condition make_error_condition( std::future_errc e );  // (since C++11)
```

Constructs an `std::error_condition` object from a value of type
`std::future_errc` as if by:

`std::error_condition(static_cast<int>(e), std::future_category())`.

### Parameters

- **e** — error code number

### Return value

A value of type `std::error_condition` that holds the error code number from `e`
associated with the error category `"future"`.

### Example

### See also

- **error_condition (C++11)** — holds a portable error code (class)
- **future_errc (C++11)** — identifies the future error codes (enum)

---
*Source: https://en.cppreference.com/w/cpp/thread/future_errc/make_error_condition*
