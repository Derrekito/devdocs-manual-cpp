# std::is_error_code_enum

```cpp
template< class T >
struct is_error_code_enum;  // (since C++11)
```

If `T` is an error code enumeration, this template provides the member constant
`value` equal `true`. For any other type, `value` is `false`.

This template may be specialized for a user-defined type to indicate that the
type is eligible for `std::error_code` and `std::error_condition` automatic
conversions.

The following classes of the standard library are an error code enum:

- `std::io_errc`
- `std::future_errc`.

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_error_code_enum_v = is_error_code_enum<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is an error code enum, `false` otherwise
  (public static member constant)

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
*Source: https://en.cppreference.com/w/cpp/error/error_code/is_error_code_enum*
