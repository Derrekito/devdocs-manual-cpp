# std::is_implicit_lifetime

```cpp
template< class T >
struct is_implicit_lifetime;  // (since C++23)
```

`std::is_implicit_lifetime` is a UnaryTypeTrait.

If `T` is an implicit-lifetime type, provides the member constant `value` equal
to `true`. For any other type, `value` is `false`.

The behavior is undefined if `T` is an incomplete type other than an array type
or (possibly cv-qualified) `void`.

The behavior of a program that adds specializations for
`std::is_implicit_lifetime` or `std::is_implicit_lifetime_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_implicit_lifetime_v = is_implicit_lifetime<T>::value;  // (since C++23)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is an implicit-lifetime type, `false`
  otherwise (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_is_implicit_lifetime` | 202302L | (C++23) |
      `std::is_implicit_lifetime`

### Example

### See also

- **is_scalar (C++11)** — checks if a type is a scalar type (class template)
- **is_array (C++11)** — checks if a type is an array type (class template)
- **is_aggregate (C++17)** — checks if a type is an aggregate type (class
  template)
- **start_lifetime_asstart_lifetime_as_array (C++23)** — implicitly creates
  objects in given storage with the object representation reused (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_implicit_lifetime*
