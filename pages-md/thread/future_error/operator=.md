# std::future_error::operator=

```cpp
future_error& operator=( const future_error& other ) noexcept;  // (since C++11)
```

Assigns the contents with those of `other`. If `*this` and `other` both have
dynamic type `std::future_error` then `std::strcmp(what(), other.what()) == 0`
after assignment.

### Parameters

- **other** — another `future_error` object to assign with

### Return value

`*this`

### Example

---
*Source: https://en.cppreference.com/w/cpp/thread/future_error/operator%3D*
