# std::swap(std::tuple)

```cpp
template< class... Types >
void swap( std::tuple<Types...>& lhs,
           std::tuple<Types...>& rhs ) noexcept(/* see below */);  // (since C++11) (until C++20)
template< class... Types >
constexpr void swap( std::tuple<Types...>& lhs,
                     std::tuple<Types...>& rhs ) noexcept(/* see below */);  // (since C++20)
template< class... Types >
constexpr void swap( const std::tuple<Types...>& lhs,
                     const std::tuple<Types...>& rhs ) noexcept(/* see below */);  // (2) (since C++23)
```

Swaps the contents of `lhs` and `rhs`. Equivalent to `lhs.swap(rhs)`.

1) This overload participates in overload resolution only if
`std::is_swappable_v<Ti>` is `true` for all i from 0 to `sizeof...(Types)`.
2) This overload participates in overload resolution only if
`std::is_swappable_v<const Ti>` is `true` for all i from 0 to
`sizeof...(Types)`.
*(since C++17)*

### Parameters

- **lhs, rhs** — tuples whose contents to swap

### Return value

(none)

### Exceptions

`noexcept` specification:

`noexcept(noexcept(lhs.swap(rhs)))`

### Example

```cpp
#include <iostream>
#include <string>
#include <tuple>

int main()
{
    std::tuple<int, std::string, float> p1{42, "ABCD", 2.71}, p2;
    p2 = std::make_tuple(10, "1234", 3.14);

    auto print_p1_p2 = [&](auto rem)
    {
        std::cout << rem
                  << "p1 = {" << std::get<0>(p1)
                  << ", "     << std::get<1>(p1)
                  << ", "     << std::get<2>(p1) << "}, "
                  << "p2 = {" << std::get<0>(p2)
                  << ", "     << std::get<1>(p2)
                  << ", "     << std::get<2>(p2) << "}\n";
    };

    print_p1_p2("Before p1.swap(p2): ");
    p1.swap(p2);
    print_p1_p2("After  p1.swap(p2): ");
    swap(p1, p2);
    print_p1_p2("After swap(p1, p2): ");
}
```

Output:

```text
Before p1.swap(p2): p1 = {42, ABCD, 2.71}, p2 = {10, 1234, 3.14}
After  p1.swap(p2): p1 = {10, 1234, 3.14}, p2 = {42, ABCD, 2.71}
After swap(p1, p2): p1 = {42, ABCD, 2.71}, p2 = {10, 1234, 3.14}
```

### See also

- **swap** — swaps the contents of two `tuple`s (public member function)
- **std::swap(std::pair) (C++11)** — specializes the `std::swap` algorithm
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/tuple/swap2*
