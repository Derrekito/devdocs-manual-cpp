# std::is_union

```cpp
template< class T >
struct is_union;  // (since C++11)
```

`std::is_union` is a UnaryTypeTrait.

Checks whether `T` is a union type. Provides the member constant `value`, which
is equal to `true` if `T` is a union type. Otherwise, `value` is equal to
`false`.

The behavior of a program that adds specializations for `std::is_union` or
`std::is_union_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_union_v = is_union<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is a union type, `false` otherwise (public
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
#include <iostream>
#include <type_traits>

struct A {};

typedef union
{
    int a;
    float b;
} B;

struct C { B d; };

int main()
{
    std::cout << std::boolalpha;
    std::cout << std::is_union<A>::value << '\n';
    std::cout << std::is_union<B>::value << '\n';
    std::cout << std::is_union<C>::value << '\n';
    std::cout << std::is_union<int>::value << '\n';
}
```

Output:

```text
false
true
false
false
```

### See also

- **is_class (C++11)** — checks if a type is a non-union class type (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_union*
