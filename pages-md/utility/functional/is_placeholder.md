# std::is_placeholder

```cpp
template< class T >
struct is_placeholder;  // (since C++11)
```

If `T` is the type of a standard placeholder `(_1, _2, _3, ...)`, then this
template is derived from `std::integral_constant<int, 1>`,
`std::integral_constant<int, 2>`, `std::integral_constant<int, 3>`,
respectively.

If `T` is not a standard placeholder type, this template is derived from
`std::integral_constant<int, 0>`.

The template may be specialized for any user-defined `T` type: the
specialization must satisfy UnaryTypeTrait with *base characteristic* of
`std::integral_constant<int, N>` with `N > 0` to indicate that `T` should be
treated as `N`th placeholder type.

`std::bind` uses `std::is_placeholder` to detect placeholders for unbound
arguments.

### Helper variable template

```cpp
template< class T >
inline constexpr int is_placeholder_v = is_placeholder<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — placeholder value or `​0​` for non-placeholder types
  (public static member constant)

### Member functions

- **operator int** — converts the object to int, returns `value` (public member
  function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — int
- **`type`** — std::integral_constant<int, value>

### Example

```cpp
#include <functional>
#include <iostream>
#include <type_traits>

struct My_2 {} my_2;

namespace std
{
    template<>
    struct is_placeholder<My_2> : public integral_constant<int, 2> {};
}

int f(int n1, int n2)
{
    return n1 + n2;
}

int main()
{
    std::cout << "Standard placeholder _5 is for the argument number "
              << std::is_placeholder_v<decltype(std::placeholders::_5)>
              << '\n';

    auto b = std::bind(f, my_2, 2);
    std::cout << "Adding 2 to 11 selected with a custom placeholder gives "
              << b(10, 11) // the first argument, namely 10, is ignored
              << '\n';
}
```

Output:

```text
Standard placeholder _5 is for the argument number 5
Adding 2 to 11 selected with a custom placeholder gives 13
```

### See also

- **bind (C++11)** — binds one or more arguments to a function object (function
  template)
- **_1, _2, _3, _4, ... (C++11)** — placeholders for the unbound arguments in a
  `std::bind` expression (constant)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/is_placeholder*
