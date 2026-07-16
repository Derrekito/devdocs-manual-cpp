# std::regex_error::operator=

```cpp
regex_error& operator=( const regex_error& other ) noexcept;  // (since C++11)
```

Assigns the contents with those of `other`. If `*this` and `other` both have
dynamic type `std::regex_error` then `std::strcmp(what(), other.what()) == 0`
after assignment.

### Parameters

- **other** — another `regex_error` object to assign with

### Return value

`*this`

### Example

---
*Source: https://en.cppreference.com/w/cpp/regex/regex_error/operator%3D*
