# operator==,!=,<,<=,>,>=,<=>(std::move_iterator)

```cpp
template< class Iterator1, class Iterator2 >
bool operator==( const std::move_iterator<Iterator1>& lhs,
                 const std::move_iterator<Iterator2>& rhs );  // (until C++17)
template< class Iterator1, class Iterator2 >
constexpr bool operator==( const std::move_iterator<Iterator1>& lhs,
                           const std::move_iterator<Iterator2>& rhs );  // (since C++17)
template< class Iterator1, class Iterator2 >
bool operator!=( const std::move_iterator<Iterator1>& lhs,
                 const std::move_iterator<Iterator2>& rhs );  // (until C++17)
template< class Iterator1, class Iterator2 >
constexpr bool operator!=( const std::move_iterator<Iterator1>& lhs,
                           const std::move_iterator<Iterator2>& rhs );  // (since C++17) (until C++20)
template< class Iterator1, class Iterator2 >
bool operator<( const std::move_iterator<Iterator1>& lhs,
                const std::move_iterator<Iterator2>& rhs );  // (until C++17)
template< class Iterator1, class Iterator2 >
constexpr bool operator<( const std::move_iterator<Iterator1>& lhs,
                          const std::move_iterator<Iterator2>& rhs );  // (since C++17)
template< class Iterator1, class Iterator2 >
bool operator<=( const std::move_iterator<Iterator1>& lhs,
                 const std::move_iterator<Iterator2>& rhs );  // (until C++17)
template< class Iterator1, class Iterator2 >
constexpr bool operator<=( const std::move_iterator<Iterator1>& lhs,
                           const std::move_iterator<Iterator2>& rhs );  // (since C++17)
template< class Iterator1, class Iterator2 >
bool operator>( const std::move_iterator<Iterator1>& lhs,
                const std::move_iterator<Iterator2>& rhs );  // (until C++17)
template< class Iterator1, class Iterator2 >
constexpr bool operator>( const std::move_iterator<Iterator1>& lhs,
                          const std::move_iterator<Iterator2>& rhs );  // (since C++17)
template< class Iterator1, class Iterator2 >
bool operator>=( const std::move_iterator<Iterator1>& lhs,
                 const std::move_iterator<Iterator2>& rhs );  // (until C++17)
template< class Iterator1, class Iterator2 >
constexpr bool operator>=( const std::move_iterator<Iterator1>& lhs,
                           const std::move_iterator<Iterator2>& rhs );  // (since C++17)
template< class Iterator1, std::three_way_comparable_with<Iterator1> Iterator2 >
constexpr std::compare_three_way_result_t<Iterator1, Iterator2>
    operator<=>( const std::move_iterator<Iterator1>& lhs,
                 const std::move_iterator<Iterator2>& rhs );  // (7) (since C++20)
```

Compares the underlying iterators.

(1-6) Only participate in overload resolution if their underlying comparison
expressions (see below) are well-formed and convertible to `bool`.
The `!=` operator is synthesized from `operator==`.
*(since C++20)*

### Parameters

- **lhs, rhs** — iterator adaptors to compare

### Return value

1) `lhs.base() == rhs.base()`

2) `!(lhs == rhs)`

3) `lhs.base() < rhs.base()`

4) `!(rhs < lhs)`

5) `rhs < lhs`

6) `!(lhs < rhs)`

7) `lhs.base() <=> rhs.base()`

### Example

```cpp
#include <compare>
#include <iostream>
#include <iterator>

int main()
{
    int a[]{9, 8, 7, 6};
    //            │  └───── x, y
    //            └──────── z

    std::move_iterator<int*>
        x{std::end(a) - 1},
        y{std::end(a) - 1},
        z{std::end(a) - 2};

    std::cout
        << std::boolalpha
        << "*x == " << *x << '\n' // 6
        << "*y == " << *y << '\n' // 6
        << "*z == " << *z << '\n' // 7
        << "x == y ? " << (x == y) << '\n' // true
        << "x != y ? " << (x != y) << '\n' // false
        << "x <  y ? " << (x <  y) << '\n' // false
        << "x <= y ? " << (x <= y) << '\n' // true
        << "x == z ? " << (x == z) << '\n' // false
        << "x != z ? " << (x != z) << '\n' // true
        << "x <  z ? " << (x <  z) << '\n' // false
        << "x <= z ? " << (x <= z) << '\n' // false
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
*x == 6
*y == 6
*z == 7
x == y ? true
x != y ? false
x <  y ? false
x <= y ? true
x == z ? false
x != z ? true
x <  z ? false
x <= z ? false
x <=> y == 0 ? true
x <=> y <  0 ? false
x <=> y >  0 ? false
x <=> z == 0 ? false
x <=> z <  0 ? false
x <=> z >  0 ? true
```

### See also

- **operator==(std::move_sentinel) (C++20)** — compares the underlying iterator
  and the underlying sentinel (function template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/move_iterator/operator_cmp*
