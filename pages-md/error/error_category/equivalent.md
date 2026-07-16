# std::error_category::equivalent

```cpp
virtual bool equivalent( int code,
                         const std::error_condition& condition ) const noexcept;  // (1) (since C++11)
virtual bool equivalent( const std::error_code& code,
                         int condition ) const noexcept;  // (2) (since C++11)
```

Checks whether error code is equivalent to an error condition for the error
category represented by `*this`.

1) Equivalent to `default_error_condition(code) == condition`.

2) Equivalent to `*this == code.category() && code.value() == condition`.

### Parameters

- **code** — specifies the error code to compare
- **condition** — specifies the error condition to compare

### Return value

`true` if the error code is equivalent to the given error condition for the
error category represented by `*this`, `false` otherwise.

---
*Source: https://en.cppreference.com/w/cpp/error/error_category/equivalent*
