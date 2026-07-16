# std::is_corresponding_member

```cpp
template< class S1, class S2, class M1, class M2 >
constexpr bool is_corresponding_member( M1 S1::* mp, M2 S2::* mq ) noexcept;  // (since C++20)
```

Determines whether `mp` and `mq` refer corresponding members in the common
initial sequence of `S1` and `S2`. The program is ill-formed if either `S1` or
`S2` is an incomplete type.

If either `S1` or `S2` is not a StandardLayoutType, or either `M1` or `M2` is
not an object type, or either `mp` or `mq` is equal to `nullptr`, the result is
always `false`.

### Parameters

- **mp, mq** — pointers-to-member to detect

### Return value

`true` if `mp` and `mq` refer corresponding members in the common initial
sequence of `S1` and `S2`, otherwise `false`.

### Notes

The type of a pointer-to-member expression `&S::m` is not always `M S::*`, where
`m` is of type `M`, because `m` may be a member inherited from a base class of
`S`. The template arguments can be specified in order to avoid potentially
surprising results.

### Example

```cpp
#include <type_traits>

struct Foo { int x; };

struct Bar
{
    int y;
    double z;
};

struct Baz : Foo, Bar {}; // not standard-layout

static_assert(
    std::is_same_v<decltype(&Baz::x), int Foo::*> == true &&
    std::is_same_v<decltype(&Baz::y), int Bar::*> == true &&
    std::is_corresponding_member(&Foo::x, &Bar::y) == true &&
    std::is_corresponding_member(&Baz::x, &Baz::y) == true &&
    std::is_corresponding_member<Baz, Baz, int, int>(&Baz::x, &Baz::y) == false
);

int main() {}
```

### See also

- **is_standard_layout (C++11)** — checks if a type is a standard-layout type
  (class template)
- **is_layout_compatible (C++20)** — checks if two types are *layout-compatible*
  (class template)
- **is_member_object_pointer (C++11)** — checks if a type is a pointer to a
  non-static member object (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_corresponding_member*
