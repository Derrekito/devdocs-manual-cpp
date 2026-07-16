# std::is_sorted

Checks whether a range is already sorted (non-descending by default) — use
it to verify an assumption before calling something that requires a sorted
range, like `std::binary_search`, `std::lower_bound`, or `std::merge`, all
of which give silently wrong answers on unsorted input instead of failing
loudly.

```cpp skip
std::is_sorted(first, last);               // uses operator<
std::is_sorted(first, last, comp);          // your ordering
std::is_sorted(policy, first, last);        // parallel        (since C++17)
std::is_sorted(policy, first, last, comp);  // parallel + comp (since C++17)
```

Available since C++11; `constexpr` since C++20.

### What you provide

- **first, last** — forward iterators over the range to check.
- **comp** — a callable answering *does `a` belong before `b`?*, taking two
  elements and returning `bool`.
- **policy** — an execution policy such as `std::execution::par` (C++17).

### Guarantees and costs

- Linear in the distance between `first` and `last`.
- Empty ranges and ranges of length one are always considered sorted.
- Parallel overloads: if a comparator throws, `std::terminate` is called;
  allocation failure throws `std::bad_alloc`.

### Gotchas

- `is_sorted` checks the *same* ordering you're about to rely on — if you
  check with `operator<` but then call `binary_search` with a custom
  `comp`, the check doesn't cover you. Use the same comparator in both
  places.

### Example

```cpp
#include <algorithm>
#include <functional>
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> v{3, 1, 4, 1, 5};
    std::cout << std::boolalpha;
    std::cout << "unsorted: " << std::is_sorted(v.begin(), v.end()) << '\n';

    std::sort(v.begin(), v.end());
    std::cout << "after sort: " << std::is_sorted(v.begin(), v.end()) << '\n';
    std::cout << "descending check: "
              << std::is_sorted(v.begin(), v.end(), std::greater<>{}) << '\n';
}
```

```text
unsorted: false
after sort: true
descending check: false
```

### Reference

Full declarations:

```cpp skip
template< class ForwardIt >
bool is_sorted( ForwardIt first, ForwardIt last );  // (since C++11) (until C++20)
template< class ForwardIt >
constexpr bool is_sorted( ForwardIt first, ForwardIt last );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt >
bool is_sorted( ExecutionPolicy&& policy, ForwardIt first, ForwardIt last );  // (2) (since C++17)
template< class ForwardIt, class Compare >
bool is_sorted( ForwardIt first, ForwardIt last, Compare comp );  // (since C++11) (until C++20)
template< class ForwardIt, class Compare >
constexpr bool is_sorted( ForwardIt first, ForwardIt last, Compare comp );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class Compare >
bool is_sorted( ExecutionPolicy&& policy, ForwardIt first, ForwardIt last,
                Compare comp );  // (4) (since C++17)
```

A sequence is sorted with respect to `comp` when, for every valid `it` and
non-negative `n`, `comp(*(it + n), *it)` is `false`. `ForwardIt` must
satisfy LegacyForwardIterator. It's implemented in terms of
`is_sorted_until`: `return std::is_sorted_until(first, last) == last;`. The
policy overloads participate in overload resolution only for genuine
execution-policy types.

### See also

- **is_sorted_until** — the largest sorted prefix subrange (C++11)
- **ranges::is_sorted** — constrained version with projections (C++20)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/is_sorted*
