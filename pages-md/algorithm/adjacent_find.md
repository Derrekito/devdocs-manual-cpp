# std::adjacent_find

Answers: *where's the first pair of equal (or predicate-matching)
neighbors?* Scans a range and returns an iterator to the first of two
consecutive elements that are equal — or, with a predicate, the first
pair for which it returns `true`. Returns `last` if no such pair exists.

```cpp skip
std::adjacent_find(first, last);              // operator==
std::adjacent_find(first, last, pred);        // custom "equal enough" test
std::adjacent_find(policy, first, last, ...);  // parallel        (since C++17)
```

Since C++20 the non-policy overloads are `constexpr`.

### What you provide

- **first, last** — forward iterators (LegacyForwardIterator).
- **pred** — a binary predicate: return `true` if two adjacent elements
  should count as a match. It must not modify its arguments.
- **policy** — an execution policy (C++17) to search with multiple
  threads.

### Guarantees and costs

- Returns an iterator `it` such that `*it == *(it + 1)` (or
  `pred(*it, *(it + 1))`); returns `last` if no such pair is found.
- Complexity: exactly `min((result - first) + 1, (last - first) - 1)`
  applications of the comparison for the non-policy overloads;
  `O(last - first)` for the parallel overloads.
- Parallel overloads: a predicate that throws calls `std::terminate` for
  standard policies; allocation failure throws `std::bad_alloc`.

### Gotchas

- A custom predicate isn't limited to equality — e.g. `std::greater<>{}`
  finds the first place a non-decreasing run breaks (see the example).
- It only compares neighbors: it won't find two equal elements that
  aren't adjacent. Sort first, or use `std::unique`'s approach, if that's
  what you actually need.

### Example

```cpp
#include <algorithm>
#include <functional>
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> v{0, 1, 2, 3, 40, 40, 41, 41, 5};

    auto i1 = std::adjacent_find(v.begin(), v.end());
    std::cout << "first equal pair at index "
              << std::distance(v.begin(), i1) << ", value " << *i1 << '\n';

    auto i2 = std::adjacent_find(v.begin(), v.end(), std::greater<int>());
    std::cout << "first decrease at index "
              << std::distance(v.begin(), i2) << ", value " << *i2 << '\n';
}
```

```text
first equal pair at index 4, value 40
first decrease at index 7, value 41
```

### Reference

Full declarations:

```cpp skip
template< class ForwardIt >
ForwardIt adjacent_find( ForwardIt first, ForwardIt last );  // (until C++20)
template< class ForwardIt >
constexpr ForwardIt adjacent_find( ForwardIt first, ForwardIt last );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt >
ForwardIt adjacent_find( ExecutionPolicy&& policy,
                         ForwardIt first, ForwardIt last );  // (2) (since C++17)
template< class ForwardIt, class BinaryPredicate >
ForwardIt adjacent_find( ForwardIt first, ForwardIt last, BinaryPredicate p );  // (until C++20)
template< class ForwardIt, class BinaryPredicate >
constexpr ForwardIt adjacent_find( ForwardIt first, ForwardIt last,
                                   BinaryPredicate p );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class BinaryPredicate >
ForwardIt adjacent_find( ExecutionPolicy&& policy,
                         ForwardIt first, ForwardIt last, BinaryPredicate p );  // (4) (since C++17)
```

`ForwardIt` must meet LegacyForwardIterator; `BinaryPredicate` must meet
the BinaryPredicate requirements.

### See also

- **unique** — removes consecutive duplicate elements from a range
- **ranges::adjacent_find** (C++20) — constrained version with
  projections

---
*Source: https://en.cppreference.com/w/cpp/algorithm/adjacent_find*
