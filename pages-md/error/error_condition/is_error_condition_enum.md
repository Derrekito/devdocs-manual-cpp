# std::is_error_condition_enum

```cpp
template< class T >
struct is_error_condition_enum;  // (since C++11)
```

If `T` is an error condition enum, this template provides the member constant
`value` equal `true`. For any other type, `value` is `false`.

This template may be specialized for a user-defined type to indicate that the
type is eligible for `std::error_condition` automatic conversions.

The following class of the standard library is an error condition enum:
`std::errc`.

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_error_condition_enum_v = is_error_condition_enum<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is an error condition enum, `false`
  otherwise (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### See also

- **is_error_code_enum (C++11)** — identifies a class as an `error_code`
  enumeration (class template)

---
*Source: https://en.cppreference.com/w/cpp/error/error_condition/is_error_condition_enum*
