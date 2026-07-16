# std::ranges::partition_point

```cpp
Call signature
template< std::forward_iterator I, std::sentinel_for<I> S,
          class Proj = std::identity,
          std::indirect_unary_predicate<std::projected<I, Proj>> Pred >
constexpr I
    partition_point( I first, S last, Pred pred, Proj proj = {} );  // (1) (since C++20)
template< ranges::forward_range R,
          class Proj = std::identity,
          std::indirect_unary_predicate<
              std::projected<ranges::iterator_t<R>, Proj>> Pred >
constexpr ranges::borrowed_iterator_t<R>
    partition_point( R&& r, Pred pred, Proj proj = {} );  // (2) (since C++20)
```

Examines the partitioned (as if by `ranges::partition`) range
`[``first``,``last``)` or `r` and locates the end of the first partition, that
is, the projected element that does not satisfy `pred` or `last` if all
projected elements satisfy `pred`.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **first, last** — iterator-sentinel defining the partially-ordered range to
  examine
- **r** — the partially-ordered range to examine
- **pred** — predicate to apply to the projected elements
- **proj** — projection to apply to the elements

### Return value

The iterator past the end of the first partition within `[``first``,``last``)`
or the iterator equal to `last` if all projected elements satisfy `pred`.

### Complexity

Given `N = ranges::distance(first, last)`, performs O(log N) applications of the
predicate `pred` and projection `proj`.

However, if sentinels don't model `std::sized_sentinel_for<I>`, the number of
iterator increments is O(N).

### Notes

This algorithm is a more general form of `ranges::lower_bound`, which can be
expressed in terms of `ranges::partition_point` with the predicate `[&](auto
const& e) { return std::invoke(pred, e, value); });`.

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

    std::ranges::partition(v, is_even);
    print_seq("After partitioning, v: ", v.cbegin(), v.cend());

    const auto pp = std::ranges::partition_point(v, is_even);
    const auto i = std::ranges::distance(v.cbegin(), pp);
    std::cout << "Partition point is at " << i << "; v[" << i << "] = " << *pp << '\n';

    print_seq("First partition (all even elements): ", v.cbegin(), pp);
    print_seq("Second partition (all odd elements): ", pp, v.cend());
}
```

Possible output:

```text
After partitioning, v: 2 4 6 8 5 3 7 1 9
Partition point is at 4; v[4] = 5
First partition (all even elements): 2 4 6 8
Second partition (all odd elements): 5 3 7 1 9
```

### See also

- **ranges::is_sorted (C++20)** — checks whether a range is sorted into
  ascending order (niebloid)
- **ranges::lower_bound (C++20)** — returns an iterator to the first element
  *not less* than the given value (niebloid)
- **partition_point (C++11)** — locates the partition point of a partitioned
  range (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/partition_point*
