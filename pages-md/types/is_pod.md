# std::is_pod

```cpp
template< class T >
struct is_pod;  // (since C++11) (deprecated in C++20)
```

`std::is_pod` is a UnaryTypeTrait.

If `T` is a POD type ("plain old data type"), provides the member constant
`value` equal to `true`. For any other type, `value` is `false`.

If std::remove_all_extents_t<T> is an incomplete type and not (possibly
cv-qualified) void, the behavior is undefined.

The behavior of a program that adds specializations for `std::is_pod` or
`std::is_pod_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_pod_v = is_pod<T>::value;  // (since C++17) (deprecated in C++20)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is a POD type, `false` otherwise (public
  static member constant)

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
static_assert(std::is_pod_v<A> == true);

class B: public A { int m; };
static_assert(std::is_pod_v<B> == false);

struct C { virtual void foo(); };
static_assert(std::is_pod_v<C> == false);

int main() {}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2015 | C++11 | `T` could be an array of incomplete class type with unknown
      bound | the behavior is undefined in this case

### See also

- **is_standard_layout (C++11)** — checks if a type is a standard-layout type
  (class template)
- **is_trivial (C++11)** — checks if a type is trivial (class template)
- **is_scalar (C++11)** — checks if a type is a scalar type (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_pod*
