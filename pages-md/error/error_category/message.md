# std::error_category::message

```cpp
virtual std::string message( int condition ) const = 0;  // (since C++11)
```

Returns a string describing the given error condition for the error category
represented by `*this`.

### Parameters

- **condition** — specifies the error condition to describe

### Return value

A string describing the given error condition.

### Exceptions

May throw implementation-defined exceptions.

---
*Source: https://en.cppreference.com/w/cpp/error/error_category/message*
