# std::is_enum

```cpp
template< class T >
struct is_enum;  // (since C++11)
```

`std::is_enum` is a UnaryTypeTrait.

Checks whether `T` is an enumeration type. Provides the member constant `value`
which is equal to `true`, if `T` is an enumeration type. Otherwise, `value` is
equal to `false`.

The behavior of a program that adds specializations for `std::is_enum` or
`std::is_enum_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_enum_v = is_enum<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is an enumeration type, `false` otherwise
  (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Example

```cpp
#include <type_traits>

struct A { enum E {}; };
static_assert(std::is_enum_v<A> == false);
static_assert(std::is_enum_v<A::E> == true);

enum E {};
static_assert(std::is_enum_v<E> == true);

enum class Ec : int {};
static_assert(std::is_enum_v<Ec> == true);

static_assert(std::is_enum_v<int> == false);

int main() {}
```

### See also

- **is_integral (C++11)** — checks if a type is an integral type (class
  template)
- **is_arithmetic (C++11)** — checks if a type is an arithmetic type (class
  template)
- **is_scalar (C++11)** — checks if a type is a scalar type (class template)
- **is_scoped_enum (C++23)** — checks if a type is a scoped enumeration type
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_enum*
