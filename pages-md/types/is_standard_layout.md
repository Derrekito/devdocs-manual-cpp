# std::is_standard_layout

```cpp
template< class T >
struct is_standard_layout;  // (since C++11)
```

`std::is_standard_layout` is a UnaryTypeTrait.

If `T` is a standard-layout type, provides the member constant `value` equal to
`true`. For any other type, `value` is `false`.

If std::remove_all_extents_t<T> is an incomplete type and not (possibly
cv-qualified) void, the behavior is undefined.

The behavior of a program that adds specializations for
`std::is_standard_layout` or `std::is_standard_layout_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_standard_layout_v = is_standard_layout<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is a standard-layout type, `false`
  otherwise (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Notes

A pointer to a standard-layout class may be converted (with reinterpret_cast) to
a pointer to its first non-static data member and vice versa.

If a standard-layout union holds two or more standard-layout structs, it is
permitted to inspect the common initial part of them.

The macro `offsetof` is only guaranteed to be usable with standard-layout
classes.

### Example

```cpp
#include <type_traits>

struct A { int m; };
static_assert(std::is_standard_layout_v<A> == true);

class B: public A { int m; };
static_assert(std::is_standard_layout_v<B> == false);

struct C { virtual void foo(); };
static_assert(std::is_standard_layout_v<C> == false);

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
- **is_pod (C++11)(deprecated in C++20)** — checks if a type is a plain-old data
  (POD) type (class template)
- **offsetof** — byte offset from the beginning of a standard-layout type to
  specified member (function macro)

---
*Source: https://en.cppreference.com/w/cpp/types/is_standard_layout*
