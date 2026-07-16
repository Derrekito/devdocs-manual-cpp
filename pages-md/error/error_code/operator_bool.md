# std::error_code::operator bool

```cpp
explicit operator bool() const noexcept;  // (since C++11)
```

Checks if the error code value is valid, i.e. non-zero.

### Parameters

(none)

### Return value

`false` if `value() == 0`, `true` otherwise.

### Notes

Although this operator is often used as a convenient shorthand to check if any
error was returned, as in `if (ec) { /* handle error */ }`, such use is not
robust: some error codes, for example, HTTP status code `200`, may indicate
success as well.

---
*Source: https://en.cppreference.com/w/cpp/error/error_code/operator_bool*
