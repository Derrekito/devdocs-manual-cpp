# std::pair<T1,T2>::swap

```cpp
void swap( pair& other ) noexcept(/* see below */);  // (since C++11) (until C++20)
constexpr void swap( pair& other ) noexcept(/* see below */);  // (since C++20)
constexpr void swap( const pair& other ) const noexcept(/* see below */);  // (2) (since C++23)
```

Swaps `first` with `other.first` and `second` with `other.second`, as if by
`using std::swap; swap(first, other.first); swap(second, other.second);`.

If either selected `swap` function call is ill-formed or does not swap the value
of the member, the behavior is undefined.
*(until C++23)*

1) The program is ill-formed if either `std::is_swappable_v<T1>` or
`std::is_swappable_v<T2>` is not `true`.
2) The program is ill-formed if either `std::is_swappable_v<const T1>` or
`std::is_swappable_v<const T2>` is not `true`.
If either selected `swap` function call does not swap the value of the member,
the behavior is undefined.
*(since C++23)*

### Parameters

- **other** — pair of values to swap

### Return value

(none)

### Exceptions

`noexcept` specification:
`noexcept( noexcept(swap(first, other.first)) && noexcept(swap(second,
other.second)) )`
In the expression above, the identifier `swap` is looked up in the same manner
as the one used by the C++17 `std::is_nothrow_swappable` trait.
*(until C++17)*

1)
`noexcept` specification:
`noexcept( std::is_nothrow_swappable_v<first_type> &&
std::is_nothrow_swappable_v<second_type> )`
2)
`noexcept` specification:
`noexcept( std::is_nothrow_swappable_v<const first_type> &&
std::is_nothrow_swappable_v<const second_type> )`
*(since C++17)*

### Example

```cpp
#include <iostream>
#include <utility>
#include <string>
int main()
{
    std::pair<int, std::string> p1(10, "test"), p2;
    p2.swap(p1);
    std::cout << "(" << p2.first << ", " << p2.second << ")\n";

#if __cpp_lib_ranges_zip >= 202110L
    // Using the C++23 const qualified swap overload
    // (swap is no longer propagating pair constness)
    int i1 = 10, i2{};
    std::string s1("test"), s2;
    const std::pair<int&, std::string&> r1(i1, s1), r2(i2, s2);
    r2.swap(r1);
    std::cout << "(" << i2 << ", " << s2 << ")\n";
#endif
}
```

Possible output:

```text
(10, test)
(10, test)
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2456 | C++11 | the `noexcept` specification is ill-formed | made to work

### See also

- **swap** — swaps the values of two objects (function template)
- **swap** — swaps the contents of two `tuple`s (public member function of
  `std::tuple<Types...>`)

---
*Source: https://en.cppreference.com/w/cpp/utility/pair/swap*
