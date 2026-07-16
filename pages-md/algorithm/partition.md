# std::partition

Reorders a range in place so every element for which a predicate returns
`true` comes before every element for which it returns `false`, and
returns an iterator to that split point. It does **not** preserve each
group's relative order — use `std::stable_partition` when that matters.

```cpp skip
std::partition(first, last, pred);           // reorder, unstable
std::partition(policy, first, last, pred);   // parallel        (since C++17)
```

Since C++20 the non-policy overload is `constexpr`.

### What you provide

- **first, last** — forward iterators (ValueSwappable and
  LegacyForwardIterator); the algorithm is more efficient when they also
  meet LegacyBidirectionalIterator.
- **pred** — a unary predicate: return `true` if the element belongs in
  the first group. It must not modify the element, and its parameter type
  must not be a non-const reference (unless move is equivalent to copy
  for that type).
- **policy** — an execution policy (C++17) to partition with multiple
  threads.

### Guarantees and costs

- Returns an iterator to the first element of the "false" group — the
  partition point.
- Exactly N applications of `pred`, where N is the distance from `first`
  to `last`. At most N/2 swaps with bidirectional iterators, at most N
  swaps otherwise.
- Not stable: elements within each group may end up in any order.
- Parallel overload: O(N log N) swaps, O(N) predicate applications; a
  predicate that throws calls `std::terminate` for standard policies, and
  allocation failure throws `std::bad_alloc`.

### Gotchas

- Because order isn't preserved, don't rely on where an element landed
  within its group — only the "before the split / after the split"
  property is guaranteed.
- Repeatedly re-partitioning sub-ranges (as in quicksort-style code) does
  a full O(N) pass each time; it isn't cheaper than one call over the
  whole range.
- Reach for `std::stable_partition` instead if code downstream depends on
  the original relative order within either group.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <vector>

int main()
{
    std::vector<int> v{0, 1, 2, 3, 4, 5, 6, 7, 8, 9};

    auto mid = std::partition(v.begin(), v.end(),
                               [](int i) { return i % 2 == 0; });

    std::cout << "partition point at index "
              << std::distance(v.begin(), mid) << ": ";
    for (int x : v)
        std::cout << x << ' ';
    std::cout << '\n';
}
```

```text
partition point at index 5: 0 8 2 6 4 5 3 7 1 9
```

### Reference

Full declarations:

```cpp skip
template< class ForwardIt, class UnaryPredicate >
ForwardIt partition( ForwardIt first, ForwardIt last, UnaryPredicate p );  // (until C++20)
template< class ForwardIt, class UnaryPredicate >
constexpr ForwardIt partition( ForwardIt first, ForwardIt last,
                               UnaryPredicate p );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class UnaryPredicate >
ForwardIt partition( ExecutionPolicy&& policy,
                     ForwardIt first, ForwardIt last, UnaryPredicate p );  // (2) (since C++17)
```

`ForwardIt` must meet ValueSwappable and LegacyForwardIterator.
`UnaryPredicate` must meet the Predicate requirements. A retroactive
defect report (LWG 498) relaxed the C++98 requirement that `first`/`last`
be LegacyBidirectionalIterator down to LegacyForwardIterator; the weaker
complexity bound above (up to N swaps instead of N/2) is what applies for
non-bidirectional iterators.

### See also

- **is_partitioned** (C++11) — check whether a range is already
  partitioned by a predicate
- **stable_partition** — same split, but preserves relative order within
  each group
- **ranges::partition** (C++20) — constrained version with projections

---
*Source: https://en.cppreference.com/w/cpp/algorithm/partition*
