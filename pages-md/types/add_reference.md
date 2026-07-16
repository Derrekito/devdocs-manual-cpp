# std::add_lvalue_reference, std::add_rvalue_reference

```cpp
template< class T >
struct add_lvalue_reference;  // (1) (since C++11)
template< class T >
struct add_rvalue_reference;  // (2) (since C++11)
```

Creates an lvalue or rvalue reference type of `T`.

1) If `T` is a function type that has no cv- or ref- qualifier or an object
   type, provides a member typedef `type` which is `T&`. If `T` is an rvalue
   reference to some type `U`, then `type` is `U&`. Otherwise, `type` is `T`.

2) If `T` is a function type that has no cv- or ref- qualifier or an object
   type, provides a member typedef `type` which is `T&&`, otherwise `type` is
   `T`.

The behavior of a program that adds specializations for any of the templates
described on this page is undefined.

### Member types

- **`type`** — reference to `T`, or `T` if not allowed

### Helper types

```cpp
template< class T >
using add_lvalue_reference_t = typename add_lvalue_reference<T>::type;  // (since C++14)
template< class T >
using add_rvalue_reference_t = typename add_rvalue_reference<T>::type;  // (since C++14)
```

### Notes

These type transformations honor reference collapse rules:

- `std::add_lvalue_reference<T&>::type` is `T&`
- `std::add_lvalue_reference<T&&>::type` is `T&`
- `std::add_rvalue_reference<T&>::type` is `T&`
- `std::add_rvalue_reference<T&&>::type` is `T&&`

The major difference to directly using `T&` is that
`std::add_lvalue_reference<void>::type` is `void`, while `void&` leads to a
compilation error.

### Possible implementation

```cpp
namespace detail
{
    template<class T>
    struct type_identity { using type = T; }; // or use std::type_identity (since C++20)

    template<class T> // Note that `cv void&` is a substitution failure
    auto try_add_lvalue_reference(int) -> type_identity<T&>;
    template<class T> // Handle T = cv void case
    auto try_add_lvalue_reference(...) -> type_identity<T>;

    template<class T>
    auto try_add_rvalue_reference(int) -> type_identity<T&&>;
    template<class T>
    auto try_add_rvalue_reference(...) -> type_identity<T>;
} // namespace detail

template<class T>
struct add_lvalue_reference
    : decltype(detail::try_add_lvalue_reference<T>(0)) {};

template<class T>
struct add_rvalue_reference
    : decltype(detail::try_add_rvalue_reference<T>(0)) {};
```

### Example

```cpp
#include <type_traits>

int main()
{
    using non_ref = int;
    using l_ref = typename std::add_lvalue_reference_t<non_ref>;
    using r_ref = typename std::add_rvalue_reference_t<non_ref>;
    using void_ref = std::add_lvalue_reference_t<void>;

    static_assert
        (  std::is_lvalue_reference_v<non_ref> == false
        && std::is_lvalue_reference_v<l_ref> == true
        && std::is_rvalue_reference_v<r_ref> == true
        && std::is_reference_v<void_ref> == false
        );
}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2101 | C++11 | These transformation traits were required to produce
      reference to cv-/ref-qualified function types. | Produce cv-/ref-qualified
      function types themselves.

### See also

- **is_reference (C++11)** — checks if a type is either an *lvalue reference* or
  *rvalue reference* (class template)
- **remove_reference (C++11)** — removes a reference from the given type (class
  template)
- **remove_cvref (C++20)** — combines `std::remove_cv` and
  `std::remove_reference` (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/add_reference*
