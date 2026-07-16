# std::regex_error::regex_error

```cpp
regex_error( std::regex_constants::error_type ecode );  // (1) (since C++11)
regex_error( const regex_error& other );  // (2) (since C++11)
```

1) Constructs a `regex_error` with a given `ecode` of type
   `std::regex_constants::error_type`.

2) Copy constructor. Initializes the contents with those of `other`. If `*this`
   and `other` both have dynamic type `std::regex_error` then
   `std::strcmp(what(), other.what()) == 0`.

### Parameters

- **ecode** — error code indicating the error raised in regular expression
  parsing
- **other** — another `regex_error` object to copy

### See also

- **error_type (C++11)** — describes different types of matching errors
  (typedef)

---
*Source: https://en.cppreference.com/w/cpp/regex/regex_error/regex_error*
