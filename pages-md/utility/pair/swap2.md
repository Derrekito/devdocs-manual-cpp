# std::swap(std::pair)

```cpp
template< class T1, class T2 >
void swap( std::pair<T1,T2>& x, std::pair<T1,T2>& y )
    noexcept(/* see below */);  // (since C++11) (until C++20)
template< class T1, class T2 >
constexpr void swap( std::pair<T1,T2>& x, std::pair<T1,T2>& y )
    noexcept(/* see below */);  // (since C++20)
template< class T1, class T2 >
constexpr void swap( const std::pair<T1,T2>& x, const std::pair<T1,T2>& y )
    noexcept(/* see below */);  // (2) (since C++23)
```

Swaps the contents of `x` and `y`. Equivalent to `x.swap(y)`.

1) This overload participates in overload resolution only if
`std::is_swappable_v<first_type> && std::is_swappable_v<second_type>` is `true`.
2) This overload participates in overload resolution only if
`std::is_swappable_v<const first_type> && std::is_swappable_v<const
second_type>` is `true`.
*(since C++17)*

### Parameters

- **x, y** — pairs whose contents to swap

### Return value

(none)

### Exceptions

`noexcept` specification:

`noexcept(noexcept(x.swap(y)))`

### Example

```cpp
#include <iostream>
#include <utility>

int main()
{
    auto p1 = std::make_pair(10, 3.14);
    auto p2 = std::pair(12, 1.23); // CTAD, since C++17

    auto print_p1_p2 = [&](auto msg) {
        std::cout << msg
                  << "p1 = {" << std::get<0>(p1)
                  << ", "     << std::get<1>(p1) << "}, "
                  << "p2 = {" << std::get<0>(p2)
                  << ", "     << std::get<1>(p2) << "}\n";
    };

    print_p1_p2("Before p1.swap(p2): ");
    p1.swap(p2);
    print_p1_p2("After  p1.swap(p2): ");
    std::swap(p1, p2);
    print_p1_p2("After swap(p1, p2): ");
}
```

Output:

```text
Before p1.swap(p2): p1 = {10, 3.14}, p2 = {12, 1.23}
After  p1.swap(p2): p1 = {12, 1.23}, p2 = {10, 3.14}
After swap(p1, p2): p1 = {10, 3.14}, p2 = {12, 1.23}
```

### See also

- **swap** — swaps the values of two objects (function template)
- **std::swap(std::tuple) (C++11)** — specializes the `std::swap` algorithm
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/pair/swap2*
