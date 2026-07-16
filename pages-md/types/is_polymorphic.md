# std::is_polymorphic

```cpp
template< class T >
struct is_polymorphic;  // (since C++11)
```

`std::is_polymorphic` is a UnaryTypeTrait.

If `T` is a polymorphic class (that is, a non-union class that declares or
inherits at least one virtual function), provides the member constant `value`
equal to `true`. For any other type, `value` is `false`.

If `T` is an incomplete non-union class type, the behavior is undefined.

The behavior of a program that adds specializations for `std::is_polymorphic` or
`std::is_polymorphic_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_polymorphic_v = is_polymorphic<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is a polymorphic class type, `false`
  otherwise (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Possible implementation

```cpp
namespace detail
{
    template<class T>
    std::true_type detect_is_polymorphic(
        decltype(dynamic_cast<const volatile void*>(static_cast<T*>(nullptr)))
    );
    template<class T>
    std::false_type detect_is_polymorphic(...);
} // namespace detail

template<class T>
struct is_polymorphic : decltype(detail::detect_is_polymorphic<T>(nullptr)) {};
```

### Example

```cpp
#include <type_traits>

struct A { int m; };
static_assert(!std::is_polymorphic_v<A>);

struct B { virtual void foo(); };
static_assert(std::is_polymorphic_v<B>);

struct C : B {};
static_assert(std::is_polymorphic_v<C>);

struct D { virtual ~D() = default; };
static_assert(std::is_polymorphic_v<D>);

// Uses inheritance, but not the virtual keyword:
struct E : A {};
static_assert(!std::is_polymorphic_v<E>);

struct F : virtual A {};
static_assert(!std::is_polymorphic_v<F>);

struct AX : A {};
struct AY : A {};
struct XY : virtual AX, virtual AY {};
static_assert(!std::is_polymorphic_v<XY>);

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
- **is_abstract (C++11)** — checks if a type is an abstract class type (class
  template)
- **has_virtual_destructor (C++11)** — checks if a type has a virtual destructor
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_polymorphic*
