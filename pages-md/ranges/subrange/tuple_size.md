# std::tuple_size<std:ranges::subrange>

```cpp
template< class I, class S, ranges::subrange_kind K >
struct tuple_size<ranges::subrange<I, S, K>>
    : std::integral_constant<std::size_t, 2> {};  // (since C++20)
```

The partial specialization of `std::tuple_size` for `std::ranges::subrange`
provides a compile-time way to obtain the number of components of a `subrange`,
which is always 2, using tuple-like syntax. It is provided for structured
binding support.

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
#include <array>
#include <iostream>
#include <iterator>
#include <ranges>

int main()
{
    static_assert(2 == std::tuple_size_v<std::ranges::subrange<int*, int*>>);

    using array5 = std::array<int, 5>;

    static_assert(2 == std::tuple_size<std::ranges::subrange<
        array5::const_iterator, array5::const_iterator>>{});

    constexpr array5 a{1, 2, 3, 4, 5};

    std::ranges::subrange sub_a1{a};

    for (std::cout << "sub_a1: { "; int e : sub_a1)
        std::cout << e << ' ';
    std::cout << "}\n";

    std::ranges::subrange sub_a2{std::next(cbegin(a)), std::prev(cend(a))};

    const auto [first, last] = sub_a2;
    std::cout << "sub_a2 size = " << std::distance(first, last) << '\n';

    for (std::cout << "sub_a2: { "; int e : sub_a2)
        std::cout << e << ' ';
    std::cout << "}\n";
}
```

Output:

```text
sub_a1: { 1 2 3 4 5 }
sub_a2 size = 3
sub_a2: { 2 3 4 }
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
- **std::tuple_size<std::array> (C++11)** — obtains the size of an `array`
  (class template specialization)
- **std::tuple_element<std::ranges::subrange> (C++20)** — obtains the type of
  the iterator or the sentinel of a `std::ranges::subrange` (class template
  specialization)

---
*Source: https://en.cppreference.com/w/cpp/ranges/subrange/tuple_size*
