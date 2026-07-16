# operator==,!=,<,<=,>,>=,<=>(std::reverse_iterator)

```cpp
template< class Iterator1, class Iterator2 >
bool operator==( const std::reverse_iterator<Iterator1>& lhs,
                 const std::reverse_iterator<Iterator2>& rhs );  // (until C++17)
template< class Iterator1, class Iterator2 >
constexpr bool operator==( const std::reverse_iterator<Iterator1>& lhs,
                           const std::reverse_iterator<Iterator2>& rhs );  // (since C++17)
template< class Iterator1, class Iterator2 >
bool operator!=( const std::reverse_iterator<Iterator1>& lhs,
                 const std::reverse_iterator<Iterator2>& rhs );  // (until C++17)
template< class Iterator1, class Iterator2 >
constexpr bool operator!=( const std::reverse_iterator<Iterator1>& lhs,
                           const std::reverse_iterator<Iterator2>& rhs );  // (since C++17)
template< class Iterator1, class Iterator2 >
bool operator<( const std::reverse_iterator<Iterator1>& lhs,
                const std::reverse_iterator<Iterator2>& rhs );  // (until C++17)
template< class Iterator1, class Iterator2 >
constexpr bool operator<( const std::reverse_iterator<Iterator1>& lhs,
                          const std::reverse_iterator<Iterator2>& rhs );  // (since C++17)
template< class Iterator1, class Iterator2 >
bool operator<=( const std::reverse_iterator<Iterator1>& lhs,
                 const std::reverse_iterator<Iterator2>& rhs );  // (until C++17)
template< class Iterator1, class Iterator2 >
constexpr bool operator<=( const std::reverse_iterator<Iterator1>& lhs,
                           const std::reverse_iterator<Iterator2>& rhs );  // (since C++17)
template< class Iterator1, class Iterator2 >
bool operator>( const std::reverse_iterator<Iterator1>& lhs,
                const std::reverse_iterator<Iterator2>& rhs );  // (until C++17)
template< class Iterator1, class Iterator2 >
constexpr bool operator>( const std::reverse_iterator<Iterator1>& lhs,
                          const std::reverse_iterator<Iterator2>& rhs );  // (since C++17)
template< class Iterator1, class Iterator2 >
bool operator>=( const std::reverse_iterator<Iterator1>& lhs,
                 const std::reverse_iterator<Iterator2>& rhs );  // (until C++17)
template< class Iterator1, class Iterator2 >
constexpr bool operator>=( const std::reverse_iterator<Iterator1>& lhs,
                           const std::reverse_iterator<Iterator2>& rhs );  // (since C++17)
template< class Iterator1, std::three_way_comparable_with<Iterator1> Iterator2 >
constexpr std::compare_three_way_result_t<Iterator1, Iterator2>
    operator<=>( const std::reverse_iterator<Iterator1>& lhs,
                 const std::reverse_iterator<Iterator2>& rhs );  // (7) (since C++20)
```

Compares the underlying iterators. Inverse comparisons are applied in order to
take into account that the iterator order is reversed.

(1-6) Only participate in overload resolution if their underlying comparison
expressions (see below) are well-formed and convertible to `bool`.
*(since C++20)*

### Parameters

- **lhs, rhs** — iterator adaptors to compare

### Return value

1) `lhs.base() == rhs.base()`

2) `lhs.base() != rhs.base()`

3) `lhs.base() > rhs.base()`

4) `lhs.base() >= rhs.base()`

5) `lhs.base() < rhs.base()`

6) `lhs.base() <= rhs.base()`

7) `rhs.base() <=> lhs.base()`

### Example

```cpp
#include <compare>
#include <iostream>
#include <iterator>

int main()
{
    int a[]{0, 1, 2, 3};
    //            ↑  └───── x, y
    //            └──────── z

    std::reverse_iterator<int*>
        x{std::rend(a) - std::size(a)},
        y{std::rend(a) - std::size(a)},
        z{std::rbegin(a) + 1};

    std::cout
        << std::boolalpha
        << "*x == " << *x << '\n' // 3
        << "*y == " << *y << '\n' // 3
        << "*z == " << *z << '\n' // 2
        << "x == y ? " << (x == y) << '\n' // true
        << "x != y ? " << (x != y) << '\n' // false
        << "x <  y ? " << (x <  y) << '\n' // false
        << "x <= y ? " << (x <= y) << '\n' // true
        << "x == z ? " << (x == z) << '\n' // false
        << "x != z ? " << (x != z) << '\n' // true
        << "x <  z ? " << (x <  z) << '\n' // true
        << "x <= z ? " << (x <= z) << '\n' // true
        << "x <=> y == 0 ? " << (x <=> y == 0) << '\n' // true
        << "x <=> y <  0 ? " << (x <=> y <  0) << '\n' // false
        << "x <=> y >  0 ? " << (x <=> y >  0) << '\n' // false
        << "x <=> z == 0 ? " << (x <=> z == 0) << '\n' // false
        << "x <=> z <  0 ? " << (x <=> z <  0) << '\n' // true
        << "x <=> z >  0 ? " << (x <=> z >  0) << '\n' // false
        ;
}
```

Output:

```text
*x == 3
*y == 3
*z == 2
x == y ? true
x != y ? false
x <  y ? false
x <= y ? false
x == z ? false
x != z ? true
x <  z ? true
x <= z ? true
x <=> y == 0 ? true
x <=> y <  0 ? false
x <=> y >  0 ? false
x <=> z == 0 ? false
x <=> z <  0 ? true
x <=> z >  0 ? false
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 280 | C++98 | an `std::reverse_iterator` could only compare with another
      `std::reverse_iterator` with the same underlying iterator type | allowed
      comparisons of `std::reverse_iterator` with different underlying iterator
      types

---
*Source: https://en.cppreference.com/w/cpp/iterator/reverse_iterator/operator_cmp*
