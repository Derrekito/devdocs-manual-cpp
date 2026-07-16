# std::tuple_element<std::array>

```cpp
template< std::size_t I, class T, std::size_t N >
struct tuple_element< I, std::array<T, N> >;  // (since C++11)
```

Provides compile-time indexed access to the type of the elements of the array
using tuple-like interface.

### Member types

- **type** — the type of elements of the array

### Possible implementation

```cpp
template<std::size_t I, class T>
struct tuple_element;

template<std::size_t I, class T, std::size_t N>
struct tuple_element<I, std::array<T,N>>
{
    using type = T;
};
```

### Example

```cpp
#include <array>
#include <tuple>
#include <type_traits>

int main()
{
    // define array and get the type of the element at position 0
    std::array<int, 10> data{0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
    using T = std::tuple_element<0, decltype(data)>::type; // int
    static_assert(std::is_same_v<T, int>);

    const auto const_data = data;
    using CT = std::tuple_element<0, decltype(const_data)>::type; // const int

    // the result of tuple_element depends on the cv-qualification of the tuple-like type
    static_assert(!std::is_same_v<T, CT>);
    static_assert(std::is_same_v<CT, const int>);
}
```

### See also

- **Structured binding (C++17)** — binds the specified names to sub-objects or
  tuple elements of the initializer
- **std::tuple_element<std::tuple> (C++11)** — obtains the type of the specified
  element (class template specialization)
- **std::tuple_element<std::pair> (C++11)** — obtains the type of the elements
  of `pair` (class template specialization)

---
*Source: https://en.cppreference.com/w/cpp/container/array/tuple_element*
