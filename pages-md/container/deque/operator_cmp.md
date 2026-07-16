# operator==,!=,<,<=,>,>=,<=>(std::deque)

```cpp
template< class T, class Alloc >
bool operator==( const std::deque<T, Alloc>& lhs,
                 const std::deque<T, Alloc>& rhs );  // (1)
template< class T, class Alloc >
bool operator!=( const std::deque<T, Alloc>& lhs,
                 const std::deque<T, Alloc>& rhs );  // (2) (until C++20)
template< class T, class Alloc >
bool operator<( const std::deque<T, Alloc>& lhs,
                const std::deque<T, Alloc>& rhs );  // (3) (until C++20)
template< class T, class Alloc >
bool operator<=( const std::deque<T, Alloc>& lhs,
                 const std::deque<T, Alloc>& rhs );  // (4) (until C++20)
template< class T, class Alloc >
bool operator>( const std::deque<T, Alloc>& lhs,
                const std::deque<T, Alloc>& rhs );  // (5) (until C++20)
template< class T, class Alloc >
bool operator>=( const std::deque<T, Alloc>& lhs,
                 const std::deque<T, Alloc>& rhs );  // (6) (until C++20)
template< class T, class Alloc >
/* see below */ operator<=>( const std::deque<T, Alloc>& lhs,
                             const std::deque<T, Alloc>& rhs );  // (7) (since C++20)
```

Compares the contents of two `deque`s.

1,2) Checks if the contents of `lhs` and `rhs` are equal, that is, they have the
   same number of elements and each element in `lhs` compares equal with the
   element in `rhs` at the same position.

3-6) Compares the contents of `lhs` and `rhs` lexicographically. The comparison
   is performed by a function equivalent to `std::lexicographical_compare`.
7) Compares the contents of `lhs` and `rhs` lexicographically. The comparison is
performed as if by calling `std::lexicographical_compare_three_way` on two
`deque`s with a function object performing *synthesized three-way comparison*
(see below). The return type is same as the result type of synthesized three-way
comparison.

Given two const E lvalues `lhs` and `rhs` as left hand operand and right hand
operand respectively (where `E` is `T`), *synthesized three-way comparison* is
defined as:

- if `std::three_way_comparable_with<E, E>` is satisfied, equivalent to `lhs <=>
  rhs`;
- otherwise, if comparing two const E lvalues by operator< is well-formed and
  the result type satisfies *`boolean-testable`*, equivalent to

```cpp
lhs < rhs ? std::weak_ordering::less :
rhs < lhs ? std::weak_ordering::greater :
            std::weak_ordering::equivalent
```

- otherwise, synthesized three-way comparison is not defined, and operator<=>
  does not participate in overload resolution.

The behavior of operator<=> is undefined if `three_way_comparable_with` or
*`boolean-testable`* is satisfied but not modeled, or operator< is used but `E`
and `<` do not establish a total order.

The `<`, `<=`, `>`, `>=`, and `!=` operators are synthesized from operator<=>
and operator== respectively.
*(since C++20)*

### Parameters

- **lhs, rhs** — `deque`s whose contents to compare

**-`T` must meet the requirements of EqualityComparable in order to use overloads (1,2).**

**-`T` must meet the requirements of LessThanComparable in order to use overloads (3-6). The ordering relation must establish total order.**

### Return value

1) `true` if the contents of the `deque`s are equal, `false` otherwise.

2) `true` if the contents of the `deque`s are not equal, `false` otherwise.

3) `true` if the contents of the `lhs` are lexicographically *less* than the
   contents of `rhs`, `false` otherwise.

4) `true` if the contents of the `lhs` are lexicographically *less* than or
   *equal* to the contents of `rhs`, `false` otherwise.

5) `true` if the contents of the `lhs` are lexicographically *greater* than the
   contents of `rhs`, `false` otherwise.

6) `true` if the contents of the `lhs` are lexicographically *greater* than or
   *equal* to the contents of `rhs`, `false` otherwise.

7) The relative order of the first pair of non-equivalent elements in `lhs` and
   `rhs` if there are such elements, `lhs.size() <=> rhs.size()` otherwise.

### Complexity

1,2) Constant if `lhs` and `rhs` are of different size, otherwise linear in the
   size of the `deque`.

3-7) Linear in the size of the `deque`.

### Example

```cpp
#include <cassert>
#include <deque>

int main()
{
    const std::deque
        a{1, 2, 3},
        b{1, 2, 3},
        c{7, 8, 9, 10};

    assert
    (""
        "Compare equal containers:" &&
        (a != b) == false &&
        (a == b) == true &&
        (a < b) == false &&
        (a <= b) == true &&
        (a > b) == false &&
        (a >= b) == true &&
        (a <=> b) != std::weak_ordering::less &&
        (a <=> b) != std::weak_ordering::greater &&
        (a <=> b) == std::weak_ordering::equivalent &&
        (a <=> b) >= 0 &&
        (a <=> b) <= 0 &&
        (a <=> b) == 0 &&

        "Compare non equal containers:" &&
        (a != c) == true &&
        (a == c) == false &&
        (a < c) == true &&
        (a <= c) == true &&
        (a > c) == false &&
        (a >= c) == false &&
        (a <=> c) == std::weak_ordering::less &&
        (a <=> c) != std::weak_ordering::equivalent &&
        (a <=> c) != std::weak_ordering::greater &&
        (a <=> c) < 0 &&
        (a <=> c) != 0 &&
        (a <=> c) <= 0 &&
    "");
}
```

---
*Source: https://en.cppreference.com/w/cpp/container/deque/operator_cmp*
