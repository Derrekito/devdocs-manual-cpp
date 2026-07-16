# std::nested_exception::nested_ptr

```cpp
std::exception_ptr nested_ptr() const noexcept;  // (since C++11)
```

Returns a pointer to the stored exception, if any.

### Parameters

(none)

### Return value

A `std::exception_ptr` to the stored exception if one is stored,
default-initialized `std::exception_ptr` otherwise.

---
*Source: https://en.cppreference.com/w/cpp/error/nested_exception/nested_ptr*
