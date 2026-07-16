# std::is_abstract

```cpp
template< class T >
struct is_abstract;  // (since C++11)
```

`std::is_abstract` is a UnaryTypeTrait.

If `T` is an abstract class (that is, a non-union class that declares or
inherits at least one pure virtual function), provides the member constant
`value` equal to `true`. For any other type, `value` is `false`.

If `T` is an incomplete non-union class type, the behavior is undefined.

The behavior of a program that adds specializations for `std::is_abstract` or
`std::is_abstract_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_abstract_v = is_abstract<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is an abstract class type, `false`
  otherwise (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Example

```cpp
#include <iostream>
#include <type_traits>

struct A { int m; };
static_assert(std::is_abstract_v<A> == false);

struct B { virtual void foo(); };
static_assert(std::is_abstract_v<B> == false);

struct C { virtual void foo() = 0; };
static_assert(std::is_abstract_v<C> == true);

struct D : C {};
static_assert(std::is_abstract_v<D> == true);

int main() {}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2015 | C++11 | the behavior was undefined if `T` is an incomplete union
      type | the base characteristic is `std::false_type` in this case

### See also

- **is_class (C++11)** — checks if a type is a non-union class type (class
  template)
- **is_polymorphic (C++11)** — checks if a type is a polymorphic class type
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_abstract*
