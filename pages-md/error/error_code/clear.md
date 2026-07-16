# std::error_code::clear

```cpp
void clear() noexcept;  // (since C++11)
```

Replaces the error code and error category with default values.

Equivalent to `*this = error_code(0, std::system_category())`.

### Parameters

(none)

### Return value

(none)

---
*Source: https://en.cppreference.com/w/cpp/error/error_code/clear*
