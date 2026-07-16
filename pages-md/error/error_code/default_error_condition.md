# std::error_code::default_error_condition

```cpp
std::error_condition default_error_condition() const noexcept;  // (since C++11)
```

Returns the default error condition for the current error value.

Equivalent to `category().default_error_condition(value())`.

### Parameters

(none)

### Return value

The default error condition for the current error value.

---
*Source: https://en.cppreference.com/w/cpp/error/error_code/default_error_condition*
