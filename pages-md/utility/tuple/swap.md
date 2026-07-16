# std::tuple<Types...>::swap

```cpp
void swap( tuple& other ) noexcept(/* see below */);  // (1) (since C++11) (constexpr since C++20)
constexpr void swap( const tuple& other ) noexcept(/* see below */) const;  // (2) (since C++23)
```

Calls `swap` (which might be `std::swap`, or might be found via ADL) for each
element in `*this` and its corresponding element in `other`.

If any selected `swap` function call is ill-formed, or does not swap the
corresponding elements of both tuples, the behavior is undefined.
*(until C++23)*

If any selected `swap` function call does not swap the corresponding elements of
both tuples, the behavior is undefined.
1) The program is ill-formed if `(std::is_swappable_v<Types> && ...)` is not
`true`.
2) The program is ill-formed if `(std::is_swappable_v<const Types> && ...)` is
not `true`.
*(since C++23)*

### Parameters

- **other** — tuple of values to swap

### Return value

(none)

### Exceptions

`noexcept` specification:
`noexcept( noexcept(swap(std::declval<T0&>>(), std::declval<T0&>())) &&
noexcept(swap(std::declval<T1&>>(), std::declval<T1&>())) &&
noexcept(swap(std::declval<T2&>>(), std::declval<T2&>())) && ... )`
In the expression above, the identifier `swap` is looked up in the same manner
as the one used by the C++17 `std::is_nothrow_swappable` trait.
*(until C++17)*

1)
`noexcept` specification:
`noexcept((std::is_nothrow_swappable_v<Types> && ...))`
2)
`noexcept` specification:
`noexcept((std::is_nothrow_swappable_v<const Types> && ...))`
*(since C++17)*

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

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2456 | C++11 | the `noexcept` specification is ill-formed | made to work

### See also

- **std::swap(std::tuple) (C++11)** — specializes the `std::swap` algorithm
  (function template)
- **swap (C++11)** — swaps the contents (public member function of
  `std::pair<T1,T2>`)

---
*Source: https://en.cppreference.com/w/cpp/utility/tuple/swap*
