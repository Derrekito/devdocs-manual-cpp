# std::is_trivial

```cpp
template< class T >
struct is_trivial;  // (since C++11)
```

`std::is_trivial` is a UnaryTypeTrait.

If `T` is a trivial type, provides the member constant `value` equal to `true`.
For any other type, `value` is `false`.

If std::remove_all_extents_t<T> is an incomplete type and not (possibly
cv-qualified) void, the behavior is undefined.

The behavior of a program that adds specializations for `std::is_trivial` or
`std::is_trivial_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_trivial_v = is_trivial<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is a trivial type, `false` otherwise
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

struct A { int m; };
static_assert(std::is_trivial_v<A> == true);

struct B { B() {} };
static_assert(std::is_trivial_v<B> == false);

int main() {}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2015 | C++11 | `T` could be an array of incomplete class type with unknown
      bound | the behavior is undefined in this case

### See also

- **is_trivially_copyable (C++11)** — checks if a type is trivially copyable
  (class template)
-
  **is_default_constructibleis_trivially_default_constructibleis_nothrow_default_constructible
  (C++11)(C++11)(C++11)** — checks if a type has a default constructor (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_trivial*
