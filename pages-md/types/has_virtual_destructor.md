# std::has_virtual_destructor

```cpp
template< class T >
struct has_virtual_destructor;  // (since C++11)
```

`std::has_virtual_destructor` is a UnaryTypeTrait.

If `T` is a type with a virtual destructor, the base characteristic is
`std::true_type`. For any other type, the base characteristic is
`std::false_type`.

If `T` is an incomplete non-union class type, the behavior is undefined.

The behavior of a program that adds specializations for
`std::has_virtual_destructor` or `std::has_virtual_destructor_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool has_virtual_destructor_v = has_virtual_destructor<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` has a virtual destructor, `false` otherwise
  (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Notes

If a class `C` has a public virtual destructor, it can be derived from, and the
derived object can be safely deleted through a pointer to the base object (GotW
#18). In this case, `std::is_polymorphic<C>::value` is `true`.

### Example

```cpp
#include <type_traits>

struct S {};
static_assert(!std::has_virtual_destructor_v<S>);

struct B { virtual ~B() {} };
static_assert(std::has_virtual_destructor_v<B>);

struct D : B { ~D() {} };
static_assert(std::has_virtual_destructor_v<D>);

int main()
{
    B* pd = new D;
    delete pd;
}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2015 | C++11 | the behavior was undefined if `T` is an incomplete union
      type | the base characteristic is `std::false_type` in this case

### See also

- **is_destructibleis_trivially_destructibleis_nothrow_destructible
  (C++11)(C++11)(C++11)** — checks if a type has a non-deleted destructor (class
  template)
- **is_polymorphic (C++11)** — checks if a type is a polymorphic class type
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/has_virtual_destructor*
