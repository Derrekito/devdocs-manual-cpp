# std::common_type<std::basic_const_iterator>

```cpp
template< class T, std::common_with<T> U >
    requires std::input_iterator<std::common_type_t<T, U>>
struct common_type<std::basic_const_iterator<T>, U>;  // (1) (since C++23)
template< class T, std::common_with<T> U >
    requires std::input_iterator<std::common_type_t<T, U>>
struct common_type<U, std::basic_const_iterator<T>>;  // (2) (since C++23)
template< class T, std::common_with<T> U >
    requires std::input_iterator<std::common_type_t<T, U>>
struct common_type<std::basic_const_iterator<T>,
                   std::basic_const_iterator<U>>;  // (3) (since C++23)
```

The common type of two `basic_const_iterator`s or a `basic_const_iterator` and
another iterator type is a `basic_const_iterator` of the common underlying type.

The common type is defined only if `T` and `U` share a common type which models
`input_iterator`.

### Member types

- **`type`** — std::basic_const_iterator<std::common_type_t<T, U>> (1-3)

### Example

### See also

- **common_type (C++11)** — determines the common type of a group of types
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/basic_const_iterator/common_type*
