# std::tuple_size(std::array)

```cpp
template< class T, std::size_t N >
struct tuple_size< std::array<T, N> > :
    std::integral_constant<std::size_t, N>
{ };  // (since C++11)
```

Provides access to the number of elements in an `std::array` as a compile-time
constant expression.

### Helper variable template

```cpp
template< class T >
inline constexpr std::size_t tuple_size_v = tuple_size<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `N`, the number of elements in the array (public static
  member constant)

### Member functions

- **operator std::size_t** — converts the object to std::size_t, returns `value`
  (public member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — std::size_t
- **`type`** — std::integral_constant<std::size_t, value>

### Example

```cpp
#include <array>

int main()
{
    auto arr = std::to_array("ABBA");
    static_assert(std::tuple_size<decltype(arr)>{} == 5);
}
```

### See also

- **Structured binding (C++17)** — binds the specified names to sub-objects or
  tuple elements of the initializer
- **tuple_size (C++11)** — obtains the number of elements of a tuple-like type
  (class template)
- **std::tuple_size<std::tuple> (C++11)** — obtains the size of `tuple` at
  compile time (class template specialization)
- **std::tuple_size<std::pair> (C++11)** — obtains the size of a `pair` (class
  template specialization)
- **std::tuple_size<std::ranges::subrange> (C++20)** — obtains the number of
  components of a `std::ranges::subrange` (class template specialization)

---
*Source: https://en.cppreference.com/w/cpp/container/array/tuple_size*
