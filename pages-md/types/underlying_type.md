# std::underlying_type

```cpp
template< class T >
struct underlying_type;  // (since C++11)
```

If `T` is a complete enumeration (enum) type, provides a member typedef `type`
that names the underlying type of `T`.

Otherwise, the behavior is undefined.
*(until C++20)*

Otherwise, if `T` is not an enumeration type, there is no member `type`.
Otherwise (`T` is an incomplete enumeration type), the program is ill-formed.
*(since C++20)*

The behavior of a program that adds specializations for `std::underlying_type`
is undefined.

### Member types

- **`type`** — the underlying type of `T`

### Helper types

```cpp
template< class T >
using underlying_type_t = typename underlying_type<T>::type;  // (since C++14)
```

### Notes

Each enumeration type has an *underlying type*, which can be

1. Specified explicitly (both scoped and unscoped enumerations);
1. Omitted, in which case it is `int` for scoped enumerations or an
   implementation-defined integral type capable of representing all values of
   the enum (for unscoped enumerations).

### Example

```cpp
#include <iostream>
#include <type_traits>

enum e1 {};
enum class e2 {};
enum class e3 : unsigned {};
enum class e4 : int {};

int main()
{
    constexpr bool e1_t = std::is_same_v<std::underlying_type_t<e1>, int>;
    constexpr bool e2_t = std::is_same_v<std::underlying_type_t<e2>, int>;
    constexpr bool e3_t = std::is_same_v<std::underlying_type_t<e3>, int>;
    constexpr bool e4_t = std::is_same_v<std::underlying_type_t<e4>, int>;

    std::cout
        << "underlying type for 'e1' is " << (e1_t ? "int" : "non-int") << '\n'
        << "underlying type for 'e2' is " << (e2_t ? "int" : "non-int") << '\n'
        << "underlying type for 'e3' is " << (e3_t ? "int" : "non-int") << '\n'
        << "underlying type for 'e4' is " << (e4_t ? "int" : "non-int") << '\n';
}
```

Possible output:

```text
underlying type for 'e1' is non-int
underlying type for 'e2' is int
underlying type for 'e3' is non-int
underlying type for 'e4' is int
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2396 | C++11 | incomplete enumeration types were allowed | complete
      enumeration type required

### See also

- **is_enum (C++11)** — checks if a type is an enumeration type (class template)
- **is_scoped_enum (C++23)** — checks if a type is a scoped enumeration type
  (class template)
- **to_underlying (C++23)** — converts an enumeration to its underlying type
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/types/underlying_type*
