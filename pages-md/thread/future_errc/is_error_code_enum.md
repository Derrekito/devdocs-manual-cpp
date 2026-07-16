# std::is_error_code_enum<std::future_errc>

```cpp
template<>
struct is_error_code_enum<std::future_errc> : std::true_type;  // (since C++11)
```

Specifies that `std::future_errc` is an error code enum. This enables
`std::error_code` automatic conversions.

### See also

- **is_error_code_enum (C++11)** — identifies a class as an `error_code`
  enumeration (class template)

---
*Source: https://en.cppreference.com/w/cpp/thread/future_errc/is_error_code_enum*
