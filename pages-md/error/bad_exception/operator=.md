# std::bad_exception::operator=

```cpp
bad_exception& operator=( const bad_exception& other ) throw();  // (until C++11)
bad_exception& operator=( const bad_exception& other ) noexcept;  // (since C++11)
```

Assigns the contents of `other`. If `*this` and `other` both have dynamic type
`std::exception` then `std::strcmp(what(), other.what()) == 0` after
assignment.(since C++11)

### Parameters

- **other** — another `bad_exception` object to assign

### Return value

`*this`.

---
*Source: https://en.cppreference.com/w/cpp/error/bad_exception/operator%3D*
