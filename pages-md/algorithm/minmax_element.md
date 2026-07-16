# std::minmax_element

Finds the smallest and greatest element in one pass, returning
`std::pair<ForwardIt, ForwardIt>{min_it, max_it}`. Prefer this over
calling `std::min_element` and `std::max_element` separately — it's
both faster and gives a subtly different tie-break for the max: on a
tie for greatest, it returns the **last** matching iterator, whereas
`std::max_element` returns the first.

```cpp skip
std::minmax_element(first, last);                // uses operator<
std::minmax_element(first, last, comp);           // your ordering
std::minmax_element(policy, first, last);         // parallel        (since C++17)
std::minmax_element(policy, first, last, comp);   // parallel + comp (since C++17)
```

Available since C++11; since C++17 the non-policy forms are
`constexpr`.

### What you provide

- **first, last** — forward iterators (works on `std::list`, unlike
  algorithms that need random access).
- **comp** — a callable answering *does `a` belong before `b`?*, same
  strict-weak-ordering rules as `std::sort`'s comparator: use
  `<`-style logic, never `<=`, and don't modify the arguments.
- **policy** — an execution policy (C++17) for parallel execution.

### Guarantees and costs

- At most `max(floor(3/2 * (N-1)), 0)` comparisons, where
  `N = std::distance(first, last)` — fewer than the roughly `2N`
  comparisons of separate `min_element` + `max_element` calls.
- Returns `{first, first}` if the range is empty.
- On a tie for smallest, returns the **first** matching iterator; on a
  tie for greatest, returns the **last** matching iterator.
- Parallel overloads: if the comparator throws, `std::terminate` is
  called; allocation failure throws `std::bad_alloc`.

### Gotchas

- The max-side tie-break (last match, not first) differs from
  `std::max_element`'s own rule — don't assume the two agree on
  which iterator you get back when duplicates are present.
- Both members of the pair are iterators; dereference (`*min`, `*max`)
  to get values.

### Example

```cpp
#include <algorithm>
#include <iostream>

int main()
{
    const auto v = {3, 9, 1, 4, 2, 5, 9};
    const auto [min, max] = std::minmax_element(begin(v), end(v));

    std::cout << "min = " << *min << ", max = " << *max << '\n';
}
```

```text
min = 1, max = 9
```

### Reference

Full declarations:

```cpp skip
template< class ForwardIt >
std::pair<ForwardIt, ForwardIt>
    minmax_element( ForwardIt first, ForwardIt last );  // (since C++11) (until C++17)
template< class ForwardIt >
constexpr std::pair<ForwardIt, ForwardIt>
    minmax_element( ForwardIt first, ForwardIt last );  // (since C++17)
template< class ExecutionPolicy, class ForwardIt >
std::pair<ForwardIt, ForwardIt>
    minmax_element( ExecutionPolicy&& policy, ForwardIt first, ForwardIt last );  // (2) (since C++17)
template< class ForwardIt, class Compare >
std::pair<ForwardIt, ForwardIt>
    minmax_element( ForwardIt first, ForwardIt last, Compare comp );  // (since C++11) (until C++17)
template< class ForwardIt, class Compare >
constexpr std::pair<ForwardIt, ForwardIt>
    minmax_element( ForwardIt first, ForwardIt last, Compare comp );  // (since C++17)
template< class ExecutionPolicy, class ForwardIt, class Compare >
std::pair<ForwardIt, ForwardIt>
    minmax_element( ExecutionPolicy&& policy,
                    ForwardIt first, ForwardIt last, Compare comp );  // (4) (since C++17)
```

`ForwardIt` must meet LegacyForwardIterator. `comp` must not modify
the objects passed to it and must accept all value-category
combinations of the dereferenced iterator's type.

### See also

- **min_element** — returns the smallest element in a range
- **max_element** — returns the largest element in a range
- **ranges::minmax_element** (C++20) — constrained version with projections

---
*Source: https://en.cppreference.com/w/cpp/algorithm/minmax_element*
