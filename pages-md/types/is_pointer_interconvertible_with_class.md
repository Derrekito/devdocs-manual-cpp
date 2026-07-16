# std::is_pointer_interconvertible_with_class

```cpp
template< class S, class M >
constexpr bool is_pointer_interconvertible_with_class( M S::* mp ) noexcept;  // (since C++20)
```

Given an object `s` of type `S`, determines whether `s.*mp` refers to a
subobject of `s` and `s` is pointer-interconvertible with its subobject `s.*mp`.
The program is ill-formed if `S` is not a complete type.

If `S` is not a StandardLayoutType, or `M` is not an object type, or `mp` is
equal to `nullptr`, the result is always `false`.

### Parameters

- **mp** — a pointer-to-member to detect

### Return value

`true` if `s.*mp` refers a subobject of `s` and `s` is pointer-interconvertible
with its subobject `s.*mp`, otherwise `false`, where `s` is an object of type
`S`.

### Notes

The type of a pointer-to-member expression `&S::m` is not always `M S::*`, where
`m` is of type `M`, because `m` may be a member inherited from a base class of
`S`. The template arguments can be specified in order to avoid potentially
surprising results.

If there is a value `mp` of type `M S::*` such that
`std::is_pointer_interconvertible_with_class(mp) == true`, then
`reinterpret_cast<M&>(s)` has well-defined result and it refers the same
subobject as `s.*mp`, where `s` is a valid lvalue of type `S`.

On common platforms, the bit pattern of `mp` is all zero if
`std::is_pointer_interconvertible_with_class(mp) == true`.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_is_pointer_interconvertible` | 201907L | (C++20) |
      Pointer-interconvertibility traits:
      `std::is_pointer_interconvertible_base_of`,
      `std::is_pointer_interconvertible_with_class`

### Example

```cpp
#include <type_traits>

struct Foo { int x; };
struct Bar { int y; };

struct Baz : Foo, Bar {}; // not standard-layout

static_assert( not std::is_same_v<decltype(&Baz::x), int Baz::*> );
static_assert( std::is_pointer_interconvertible_with_class(&Baz::x) );
static_assert( not std::is_pointer_interconvertible_with_class<Baz, int>(&Baz::x) );

int main() { }
```

### See also

- **is_standard_layout (C++11)** — checks if a type is a standard-layout type
  (class template)
- **is_member_object_pointer (C++11)** — checks if a type is a pointer to a
  non-static member object (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_pointer_interconvertible_with_class*
