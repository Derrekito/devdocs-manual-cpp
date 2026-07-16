# std::is_unbounded_array

```cpp
template< class T >
struct is_unbounded_array;  // (since C++20)
```

`std::is_unbounded_array` is a UnaryTypeTrait.

Checks whether `T` is an arrays of unknown bound. Provides the member constant
`value` which is equal to `true`, if `T` is an array type of unknown bound.
Otherwise, `value` is equal to `false`.

The behavior of a program that adds specializations for
`std::is_unbounded_array` or `std::is_unbounded_array_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_unbounded_array_v = is_unbounded_array<T>::value;  // (since C++20)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is an array type of unknown bound., `false`
  otherwise (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Possible implementation

```cpp
template<class T>
struct is_unbounded_array: std::false_type {};

template<class T>
struct is_unbounded_array<T[]> : std::true_type {};
```

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_bounded_array_traits` | 201902L | (C++20) |
      `std::is_bounded_array`, `std::is_unbounded_array`

### Example

```cpp
#include <type_traits>

class A {};

static_assert
(""
    && std::is_unbounded_array_v<A> == false
    && std::is_unbounded_array_v<A[]> == true
    && std::is_unbounded_array_v<A[3]> == false
    && std::is_unbounded_array_v<float> == false
    && std::is_unbounded_array_v<int> == false
    && std::is_unbounded_array_v<int[]> == true
    && std::is_unbounded_array_v<int[3]> == false
);

int main() {}
```

### See also

- **is_array (C++11)** — checks if a type is an array type (class template)
- **is_bounded_array (C++20)** — checks if a type is an array type of known
  bound (class template)
- **extent (C++11)** — obtains the size of an array type along a specified
  dimension (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_unbounded_array*
