# std::reference_constructs_from_temporary

```cpp
template< class T, class U >
struct reference_constructs_from_temporary;  // (since C++23)
```

Let `V` be `std::remove_cv_t<U>` if `U` is a scalar type or *cv* `void`, or `U`
otherwise. If `T` is a reference type, and given a hypothetic expression `e`
such that `decltype(e)` is `V`, the variable definition `T ref(e);` is
well-formed and binds a temporary object to `ref`, then provides the member
constant `value` equal to `true`. Otherwise, `value` is `false`.

If `T` is an lvalue reference type to a const- but not volatile-qualified object
type or an rvalue reference type, both `std::remove_reference_t<T>` and
`std::remove_reference_t<U>` shall be complete types, *cv* `void`, or an arrays
of unknown bound; otherwise the behavior is undefined.

If an instantiation of a template above depends, directly or indirectly, on an
incomplete type, and that instantiation could yield a different result if that
type were hypothetically completed, the behavior is undefined.

The behavior of a program that adds specializations for
`std::reference_constructs_from_temporary` or
`std::reference_constructs_from_temporary_v` is undefined.

### Helper variable template

```cpp
template< class T, class U >
inline constexpr bool reference_constructs_from_temporary_v =
    std::reference_constructs_from_temporary<T, U>::value;  // (since C++23)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is a reference type, a `U` value can be
  bound to `T` in direct-initialization, and a temporary object would be bound
  to the reference, `false` otherwise (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Notes

`std::reference_constructs_from_temporary` can be used for rejecting some cases
that always produce dangling references.

It is also possible to use member initializer list to reject binding a temporary
object to a reference if the compiler has implemented CWG1696.

### Example

```cpp
#include <type_traits>

static_assert(std::reference_constructs_from_temporary_v<int&&, int> == true);
static_assert(std::reference_constructs_from_temporary_v<const int&, int> == true);
static_assert(std::reference_constructs_from_temporary_v<int&&, int&&> == false);
static_assert(std::reference_constructs_from_temporary_v<const int&, int&&> == false);
static_assert(std::reference_constructs_from_temporary_v<int&&, long&&> == true);
static_assert(std::reference_constructs_from_temporary_v<int&&, long> == true);

int main() {}
```

### See also

- **is_constructibleis_trivially_constructibleis_nothrow_constructible
  (C++11)(C++11)(C++11)** — checks if a type has a constructor for specific
  arguments (class template)
- **(constructor)** — constructs a new `tuple` (public member function of
  `std::tuple<Types...>`)
- **(constructor)** — constructs new pair (public member function of
  `std::pair<T1,T2>`)
- **make_from_tuple (C++17)** — construct an object with a tuple of arguments
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/types/reference_constructs_from_temporary*
