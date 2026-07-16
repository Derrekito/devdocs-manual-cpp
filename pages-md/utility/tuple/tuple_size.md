# std::tuple_size<std::tuple>

```cpp
template< class... Types >
struct tuple_size< std::tuple<Types...> >
    : std::integral_constant<std::size_t, sizeof...(Types)> { };  // (since C++11)
```

Provides access to the number of elements in a tuple as a compile-time constant
expression.

### Helper variable template

```cpp
template< class T >
inline constexpr std::size_t tuple_size_v = tuple_size<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `sizeof...(Types)` (public static member constant)

### Member functions

- **operator std::size_t** — converts the object to std::size_t, returns `value`
  (public member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — std::size_t
- **`type`** — std::integral_constant<std::size_t, value>

### Example

```cpp
#include <iostream>
#include <tuple>

template <class T>
void test(T value)
{
    int a[std::tuple_size_v<T>]; // can be used at compile time

    std::cout << std::tuple_size<T>{} << ' ' // or at run time
              << sizeof a << ' '
              << sizeof value << '\n';
}

int main()
{
    test(std::make_tuple(1, 2, 3.14));
}
```

Possible output:

```text
3 12 16
```

### See also

- **Structured binding (C++17)** — binds the specified names to sub-objects or
  tuple elements of the initializer
- **tuple_size (C++11)** — obtains the number of elements of a tuple-like type
  (class template)
- **std::tuple_size<std::pair> (C++11)** — obtains the size of a `pair` (class
  template specialization)
- **std::tuple_size<std::array> (C++11)** — obtains the size of an `array`
  (class template specialization)
- **std::tuple_size<std::ranges::subrange> (C++20)** — obtains the number of
  components of a `std::ranges::subrange` (class template specialization)
- **get(std::tuple) (C++11)** — tuple accesses specified element (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/utility/tuple/tuple_size*
