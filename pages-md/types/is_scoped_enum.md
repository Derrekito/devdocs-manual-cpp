# std::is_scoped_enum

```cpp
template< class T >
struct is_scoped_enum;  // (since C++23)
```

`std::is_scoped_enum` is a UnaryTypeTrait.

Checks whether `T` is a scoped enumeration type. Provides the member constant
`value` which is equal to `true`, if `T` is a scoped enumeration type.
Otherwise, `value` is equal to `false`.

The behavior of a program that adds specializations for `std::is_scoped_enum` or
`std::is_scoped_enum_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_scoped_enum_v = is_scoped_enum<T>::value;  // (since C++23)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is a scoped enumeration type, `false`
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
  `__cpp_lib_is_scoped_enum` | 202011L | (C++23) | `std::is_scoped_enum`

### Possible implementation

```cpp
template<typename E>
struct is_scoped_enum : std::bool_constant<requires
{
    requires std::is_enum_v<E>;
    requires !std::is_convertible_v<E, std::underlying_type_t<E>>;
}>
{};
```

### Example

```cpp
#include <type_traits>

class A {};

enum E {};

enum struct Es { oz };

enum class Ec : int {};

int main()
{
    static_assert(std::is_scoped_enum_v<A> == false);
    static_assert(std::is_scoped_enum_v<E> == false);
    static_assert(std::is_scoped_enum_v<Es> == true);
    static_assert(std::is_scoped_enum_v<Ec> == true);
    static_assert(std::is_scoped_enum_v<int> == false);
}
```

### See also

- **is_integral (C++11)** — checks if a type is an integral type (class
  template)
- **is_arithmetic (C++11)** — checks if a type is an arithmetic type (class
  template)
- **is_scalar (C++11)** — checks if a type is a scalar type (class template)
- **is_enum (C++11)** — checks if a type is an enumeration type (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_scoped_enum*
