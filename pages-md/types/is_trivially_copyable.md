# std::is_trivially_copyable

```cpp
template< class T >
struct is_trivially_copyable;  // (since C++11)
```

`std::is_trivially_copyable` is a UnaryTypeTrait.

If `T` is a trivially copyable type, provides the member constant `value` equal
to `true`. For any other type, `value` is `false`.

If std::remove_all_extents_t<T> is an incomplete type and not (possibly
cv-qualified) void, the behavior is undefined.

The behavior of a program that adds specializations for
`std::is_trivially_copyable` or `std::is_trivially_copyable_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_trivially_copyable_v = is_trivially_copyable<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is a trivially copyable type, `false`
  otherwise (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Notes

Objects of trivially-copyable types that are not potentially-overlapping
subobjects are the only C++ objects that may be safely copied with `std::memcpy`
or serialized to/from binary files with `std::ofstream::write()` /
`std::ifstream::read()`.

### Example

```cpp
#include <type_traits>

struct A { int m; };
static_assert(std::is_trivially_copyable_v<A> == true);

struct B { B(B const&) {} };
static_assert(std::is_trivially_copyable_v<B> == false);

struct C { virtual void foo(); };
static_assert(std::is_trivially_copyable_v<C> == false);

struct D
{
    int m;

    D(D const&) = default; // -> trivially copyable
    D(int x) : m(x + 1) {}
};
static_assert(std::is_trivially_copyable_v<D> == true);

int main() {}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2015 | C++11 | `T` could be an array of incomplete class type with unknown
      bound | the behavior is undefined in this case

### See also

- **is_trivial (C++11)** — checks if a type is trivial (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_trivially_copyable*
