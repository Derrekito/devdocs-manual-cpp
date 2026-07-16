# std::get(std::tuple)

```cpp
template< std::size_t I, class... Types >
typename std::tuple_element<I, tuple<Types...> >::type&
    get( tuple<Types...>& t ) noexcept;  // (1) (since C++11) (constexpr since C++14)
template< std::size_t I, class... Types >
typename std::tuple_element<I, tuple<Types...> >::type&&
    get( tuple<Types...>&& t ) noexcept;  // (2) (since C++11) (constexpr since C++14)
template< std::size_t I, class... Types >
typename std::tuple_element<I, tuple<Types...> >::type const&
    get( const tuple<Types...>& t ) noexcept;  // (3) (since C++11) (constexpr since C++14)
template< std::size_t I, class... Types >
typename std::tuple_element<I, tuple<Types...> >::type const&&
    get( const tuple<Types...>&& t ) noexcept;  // (4) (since C++11) (constexpr since C++14)
template< class T, class... Types >
constexpr T& get( tuple<Types...>& t ) noexcept;  // (5) (since C++14)
template< class T, class... Types >
constexpr T&& get( tuple<Types...>&& t ) noexcept;  // (6) (since C++14)
template< class T, class... Types >
constexpr const T& get( const tuple<Types...>& t ) noexcept;  // (7) (since C++14)
template< class T, class... Types >
constexpr const T&& get( const tuple<Types...>&& t ) noexcept;  // (8) (since C++14)
```

1-4) Extracts the `I`th element from the tuple. `I` must be an integer value in
   `[``​0​``,``sizeof...(Types)``)`.

5-8) Extracts the element of the tuple `t` whose type is `T`. Fails to compile
   unless the tuple has exactly one element of that type.

### Parameters

- **t** — tuple whose contents to extract

### Return value

A reference to the selected element of `t`.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_tuples_by_type` | 201304L | (C++14) | Addressing tuples by type

### Example

```cpp
#include <iostream>
#include <string>
#include <tuple>

int main()
{
    auto t = std::make_tuple(1, "Foo", 3.14);

    // Index-based access
    std::cout << "( " << std::get<0>(t)
              << ", " << std::get<1>(t)
              << ", " << std::get<2>(t)
              << " )\n";

    // Type-based access (C++14 or later)
    std::cout << "( " << std::get<int>(t)
              << ", " << std::get<const char*>(t)
              << ", " << std::get<double>(t)
              << " )\n";

    // Note: std::tie and structured binding may also be used to decompose a tuple.
}
```

Output:

```text
( 1, Foo, 3.14 )
( 1, Foo, 3.14 )
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2485 | C++11 (by index) C++14 (by type) | there are no overloads for const
      tuple&& | the overloads are added

### See also

- **Structured binding (C++17)** — binds the specified names to sub-objects or
  tuple elements of the initializer
- **get(std::array) (C++11)** — accesses an element of an `array` (function
  template)
- **get(std::pair) (C++11)** — accesses an element of a `pair` (function
  template)
- **get(std::variant) (C++17)** — reads the value of the variant given the index
  or the type (if the type is unique), throws on error (function template)
- **get(std::ranges::subrange) (C++20)** — obtains iterator or sentinel from a
  `std::ranges::subrange` (function template)
- **get(std::complex) (C++26)** — obtains a reference to real or imaginary part
  from a `std::complex` (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/tuple/get*
