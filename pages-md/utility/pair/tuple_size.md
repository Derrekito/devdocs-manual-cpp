# std::tuple_size<std::pair>

```cpp
template< class T1, class T2 >
struct tuple_size<std::pair<T1, T2>>
    : std::integral_constant<std::size_t, 2> { };  // (since C++11)
```

The partial specialization of `std::tuple_size` for pairs provides a
compile-time way to obtain the number of elements in a pair, which is always 2,
using tuple-like syntax.

## Inherited from std::integral_constant

### Member constants

- **value [static]** — the constant value 2 (public static member constant)

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
#include <utility>

template<class T>
void test([[maybe_unused]]T t)
{
    [[maybe_unused]]
    int a[std::tuple_size<T>::value]; // can be used at compile time
    std::cout << std::tuple_size<T>::value << '\n'; // or at run time
}

int main()
{
    test(std::make_tuple(1, 2, 3.14));
    test(std::make_pair(1, 3.14));
}
```

Output:

```text
3
2
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2313 | C++11 | specializations for `pair` were not required to be derived
      from `integral_constant` | required

### See also

- **Structured binding (C++17)** — binds the specified names to sub-objects or
  tuple elements of the initializer
- **tuple_size (C++11)** — obtains the number of elements of a tuple-like type
  (class template)
- **std::tuple_size<std::array> (C++11)** — obtains the size of an `array`
  (class template specialization)
- **std::tuple_size<std::tuple> (C++11)** — obtains the size of `tuple` at
  compile time (class template specialization)
- **std::tuple_size<std::ranges::subrange> (C++20)** — obtains the number of
  components of a `std::ranges::subrange` (class template specialization)
- **std::tuple_element<std::pair> (C++11)** — obtains the type of the elements
  of `pair` (class template specialization)

---
*Source: https://en.cppreference.com/w/cpp/utility/pair/tuple_size*
