# std::partition_point

```cpp
template< class ForwardIt, class UnaryPredicate >
ForwardIt partition_point( ForwardIt first, ForwardIt last, UnaryPredicate p );  // (since C++11) (until C++20)
template< class ForwardIt, class UnaryPredicate >
constexpr ForwardIt partition_point( ForwardIt first, ForwardIt last, UnaryPredicate p );  // (since C++20)
```

Examines the partitioned (as if by `std::partition`) range
`[``first``,``last``)` and locates the end of the first partition, that is, the
first element that does not satisfy `p` or `last` if all elements satisfy `p`.

### Parameters

- **first, last** — the partitioned range of elements to examine
- **p** — unary predicate which returns ​`true` for the elements found in the
  beginning of the range. The expression `p(v)` must be convertible to `bool`
  for every argument `v` of type (possibly const) `VT`, where `VT` is the value
  type of `ForwardIt`, regardless of value category, and must not modify `v`.
  Thus, a parameter type of `VT&`is not allowed, nor is `VT` unless for `VT` a
  move is equivalent to a copy(since C++11). ​

**Type requirements**

**-`ForwardIt` must meet the requirements of LegacyForwardIterator.**

**-`UnaryPredicate` must meet the requirements of Predicate.**

### Return value

The iterator past the end of the first partition within `[``first``,``last``)`
or `last` if all elements satisfy `p`.

### Complexity

Given `N = std::distance(first, last)`, performs O(log N) applications of the
predicate `p`.

However, for non-LegacyRandomAccessIterators, the number of iterator increments
is O(N).

### Notes

This algorithm is a more general form of `std::lower_bound`, which can be
expressed in terms of `std::partition_point` with the predicate `[&](auto const&
e) { return e < value; });`.

### Possible implementation

```cpp
template<class ForwardIt, class UnaryPredicate>
constexpr //< since C++20
ForwardIt partition_point(ForwardIt first, ForwardIt last, UnaryPredicate p)
{
    for (auto length = std::distance(first, last); 0 < length; )
    {
        auto half = length / 2;
        auto middle = std::next(first, half);
        if (p(*middle))
        {
            first = std::next(middle);
            length -= (half + 1);
        }
        else
            length = half;
    }

    return first;
}
```

### Example

```cpp
#include <algorithm>
#include <array>
#include <iostream>
#include <iterator>

auto print_seq = [](auto rem, auto first, auto last)
{
    for (std::cout << rem; first != last; std::cout << *first++ << ' ') {}
    std::cout << '\n';
};

int main()
{
    std::array v {1, 2, 3, 4, 5, 6, 7, 8, 9};

    auto is_even = [](int i) { return i % 2 == 0; };

    std::partition(v.begin(), v.end(), is_even);
    print_seq("After partitioning, v: ", v.cbegin(), v.cend());

    const auto pp = std::partition_point(v.cbegin(), v.cend(), is_even);
    const auto i = std::distance(v.cbegin(), pp);
    std::cout << "Partition point is at " << i << "; v[" << i << "] = " << *pp << '\n';

    print_seq("First partition (all even elements): ", v.cbegin(), pp);
    print_seq("Second partition (all odd elements): ", pp, v.cend());
}
```

Possible output:

```text
After partitioning, v: 8 2 6 4 5 3 7 1 9
Partition point is at 4; v[4] = 5
First partition (all even elements): 8 2 6 4
Second partition (all odd elements): 5 3 7 1 9
```

### See also

- **findfind_iffind_if_not (C++11)** — finds the first element satisfying
  specific criteria (function template)
- **is_sorted (C++11)** — checks whether a range is sorted into ascending order
  (function template)
- **lower_bound** — returns an iterator to the first element *not less* than the
  given value (function template)
- **ranges::partition_point (C++20)** — locates the partition point of a
  partitioned range (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/partition_point*
