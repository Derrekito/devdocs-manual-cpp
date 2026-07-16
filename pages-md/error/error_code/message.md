# std::error_code::message

```cpp
std::string message() const;  // (since C++11)
```

Returns the message corresponding to the current error code value and category.

Equivalent to `category().message(value())`.

### Parameters

(none)

### Return value

The error message corresponding to the current error code value and category.

### Exceptions

May throw implementation-defined exceptions.

---
*Source: https://en.cppreference.com/w/cpp/error/error_code/message*
