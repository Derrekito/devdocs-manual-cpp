# std::is_layout_compatible

```cpp
template< class T, class U >
struct is_layout_compatible;  // (since C++20)
```

If `T` and `U` are *layout-compatible* types, provides the member constant
`value` equal to `true`. Otherwise `value` is `false`.

Every type is layout-compatible with its any cv-qualified versions, even if it
is not an object type.

`T` and `U` shall each be a complete type, (possibly cv-qualified) void, or an
array of unknown bound. Otherwise, the behavior is undefined.

If an instantiation of a template above depends, directly or indirectly, on an
incomplete type, and that instantiation could yield a different result if that
type were hypothetically completed, the behavior is undefined.

The behavior of a program that adds specializations for
`std::is_layout_compatible` or `std::is_layout_compatible_v` is undefined.

### Helper variable template

```cpp
template< class T, class U >
inline constexpr bool is_layout_compatible_v = is_layout_compatible<T, U>::value;  // (since C++20)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` and `U` are layout-compatible, `false`
  otherwise (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Notes

A signed integer type and its unsigned counterpart are not layout-compatible.
`char` is layout-compatible with neither `signed char` nor `unsigned char`.

Similar types are not layout-compatible if they are not the same type after
ignoring top-level cv-qualification.

An enumeration type and its underlying type are not layout-compatible.

Array types of layout-compatible but different element types (ignoring
cv-qualification) are not layout-compatible, even if they are of equal length.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_is_layout_compatible` | 201907L | (C++20) |
      `std::is_layout_compatible`

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <type_traits>

struct Foo
{
    int x;
    char y;
};

struct FooNua
{
    int x;
    [[no_unique_address]] char y;
};

class Bar
{
    const int u = 42;
    volatile char v = '*';
};

enum E0 : int {};
enum class E1 : int {};

static_assert
(
    std::is_layout_compatible_v<const void, volatile void> == true  and
    std::is_layout_compatible_v<Foo, Bar>                  == true  and
    std::is_layout_compatible_v<Foo[2], Bar[2]>            == false and
    std::is_layout_compatible_v<int, E0>                   == false and
    std::is_layout_compatible_v<E0, E1>                    == true  and
    std::is_layout_compatible_v<long, unsigned long>       == false and
    std::is_layout_compatible_v<char*, const char*>        == false and
    std::is_layout_compatible_v<char*, char* const>        == true  and
    std::is_layout_compatible_v<Foo, FooNua>               == false // Note [1]
);

// [1] MSVC erroneously fails this assert

int main() {}
```

### See also

- **is_standard_layout (C++11)** — checks if a type is a standard-layout type
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_layout_compatible*
