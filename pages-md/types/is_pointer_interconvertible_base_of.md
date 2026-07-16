# std::is_pointer_interconvertible_base_of

```cpp
template< class Base, class Derived >
struct is_pointer_interconvertible_base_of;  // (since C++20)
```

If `Derived` is unambiguously derived from `Base` and every `Derived` object is
pointer-interconvertible with its `Base` subobject, or if both are the same
non-union class (in both cases ignoring cv-qualification), provides the member
constant `value` equal to `true`. Otherwise `value` is `false`.

If both `Base` and `Derived` are non-union class types, and they are not the
same type (ignoring cv-qualification), `Derived` shall be a complete type;
otherwise the behavior is undefined.

The behavior of a program that adds specializations for
`std::is_pointer_interconvertible_base_of` or
`std::is_pointer_interconvertible_base_of_v` is undefined.

### Helper variable template

```cpp
template< class Base, class Derived >
inline constexpr bool is_pointer_interconvertible_base_of_v =
    is_pointer_interconvertible_base_of<Base, Derived>::value;  // (since C++20)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `Derived` is unambiguously derived from `Base`
  and every `Derived` object is pointer-interconvertible with its `Base`
  subobject, or if both are the same non-union class (in both cases ignoring
  cv-qualification), `false` otherwise (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Notes

`std::is_pointer_interconvertible_base_of_v<T, U>` may be `true` even if `T` is
a private or protected base class of `U`.

Let

- `U` be a complete object type,
- `T` be a complete object type with cv-qualification not less than `U`,
- `u` be any valid lvalue of `U`,

`reinterpret_cast<T&>(u)` always has well-defined result if
`std::is_pointer_interconvertible_base_of_v<T, U>` is `true`.

If `T` and `U` are not the same type (ignoring cv-qualification) and `T` is a
pointer-interconvertible base class of `U`, then both
`std::is_standard_layout_v<T>` and `std::is_standard_layout_v<U>` are `true`.

If `T` is standard layout class type, then all base classes of `T` (if any) are
pointer-interconvertible base class of `T`.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_is_pointer_interconvertible` | 201907L | (C++20) |
      Pointer-interconvertibility traits:
      `std::is_pointer_interconvertible_base_of`,
      `std::is_pointer_interconvertible_with_class`

### Example

```cpp
#include <type_traits>

struct Foo {};

struct Bar {};

class Baz : Foo, public Bar { int x; };

class NonStdLayout : public Baz { int y; };

static_assert(std::is_pointer_interconvertible_base_of_v<Bar, Baz>);
static_assert(std::is_pointer_interconvertible_base_of_v<Foo, Baz>);
static_assert(not std::is_pointer_interconvertible_base_of_v<Baz, NonStdLayout>);
static_assert(std::is_pointer_interconvertible_base_of_v<NonStdLayout, NonStdLayout>);

int main() {}
```

### See also

- **is_base_of (C++11)** — checks if a type is derived from the other type
  (class template)
- **is_empty (C++11)** — checks if a type is a class (but not union) type and
  has no non-static data members (class template)
- **is_standard_layout (C++11)** — checks if a type is a standard-layout type
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_pointer_interconvertible_base_of*
