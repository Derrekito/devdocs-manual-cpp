# std::is_scalar

```cpp
template< class T >
struct is_scalar;  // (since C++11)
```

`std::is_scalar` is a UnaryTypeTrait.

If `T` is a scalar type, provides the member constant `value` equal `true`. For
any other type, `value` is `false`.

The behavior of a program that adds specializations for `std::is_scalar` or
`std::is_scalar_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_scalar_v = is_scalar<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is a scalar type, `false` otherwise (public
  static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Notes

Each individual memory location in the C++ memory model, including the hidden
memory locations used by language features (e.g. virtual table pointer), has
scalar type (or is a sequence of adjacent bit-fields of non-zero length).
Sequencing of side-effects in expression evaluation, inter-thread
synchronization, and dependency ordering are all defined in terms of individual
scalar objects.

### Possible implementation

```cpp
template<class T>
struct is_scalar : std::integral_constant<bool, std::is_arithmetic<T>::value
                                             || std::is_enum<T>::value
                                             || std::is_pointer<T>::value
                                             || std::is_member_pointer<T>::value
                                             || std::is_null_pointer<T>::value>
{};
```

### Example

```cpp
#include <iostream>
#include <type_traits>
#include <typeinfo>
#include <utility>

template<typename Head, typename... Tail>
void are_scalars(Head&& head, Tail&&... tail)
{
    using T = std::decay_t<decltype(head)>;

    std::cout << typeid(T).name() << " is "
              << (std::is_scalar_v<T> ? "" : "not ")
              << "a scalar\n";

    if constexpr (sizeof... (Tail))
    {
        are_scalars(std::forward<decltype(tail)>(tail)...);
    }
}

int main()
{
    struct S { int m; } s;
    int S::* mp = &S::m;
    enum class E { e };

    are_scalars(42, 3.14, E::e, "str", mp, nullptr, s);
}
```

Possible output:

```text
int is a scalar
double is a scalar
main::E is a scalar
char const* is a scalar
int main::S::* is a scalar
nullptr is a scalar
main::S is not a scalar
```

### See also

- **is_arithmetic (C++11)** — checks if a type is an arithmetic type (class
  template)
- **is_enum (C++11)** — checks if a type is an enumeration type (class template)
- **is_pointer (C++11)** — checks if a type is a pointer type (class template)
- **is_member_pointer (C++11)** — checks if a type is a pointer to a non-static
  member function or object (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_scalar*
