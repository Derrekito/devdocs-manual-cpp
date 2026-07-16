# std::is_array

```cpp
template< class T >
struct is_array;  // (since C++11)
```

`std::is_array` is a UnaryTypeTrait.

Checks whether `T` is an array type. Provides the member constant `value` which
is equal to `true`, if `T` is an array type. Otherwise, `value` is equal to
`false`.

The behavior of a program that adds specializations for `std::is_array` or
`std::is_array_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_array_v = is_array<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is an array type, `false` otherwise (public
  static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Possible implementation

```cpp
template<class T>
struct is_array : std::false_type {};

template<class T>
struct is_array<T[]> : std::true_type {};

template<class T, std::size_t N>
struct is_array<T[N]> : std::true_type {};
```

### Example

```cpp
#include <array>
#include <iostream>
#include <type_traits>

class A {};

int main()
{
    std::cout << std::boolalpha;
    std::cout << std::is_array<A>::value << '\n';
    std::cout << std::is_array<A[]>::value << '\n';
    std::cout << std::is_array<A[3]>::value << '\n';
    std::cout << std::is_array<float>::value << '\n';
    std::cout << std::is_array<int>::value << '\n';
    std::cout << std::is_array<int[]>::value << '\n';
    std::cout << std::is_array<int[3]>::value << '\n';
    std::cout << std::is_array<std::array<int, 3>>::value << '\n';
}
```

Output:

```text
false
true
true
false
false
true
true
false
```

### See also

- **is_bounded_array (C++20)** — checks if a type is an array type of known
  bound (class template)
- **is_unbounded_array (C++20)** — checks if a type is an array type of unknown
  bound (class template)
- **rank (C++11)** — obtains the number of dimensions of an array type (class
  template)
- **extent (C++11)** — obtains the size of an array type along a specified
  dimension (class template)
- **remove_extent (C++11)** — removes one extent from the given array type
  (class template)
- **remove_all_extents (C++11)** — removes all extents from the given array type
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_array*
