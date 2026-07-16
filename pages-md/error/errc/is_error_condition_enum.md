# std::is_error_condition_enum<std::errc>

```cpp
template<>
struct is_error_condition_enum<std::errc> : std::true_type;  // (since C++11)
```

Specifies that `std::errc` is an error condition enum. This enables implicit
conversion to `std::error_condition`.

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### See also

- **is_error_condition_enum (C++11)** — identifies an enumeration as an
  `std::error_condition` (class template)

---
*Source: https://en.cppreference.com/w/cpp/error/errc/is_error_condition_enum*
