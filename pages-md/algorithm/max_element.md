# std::max_element

Finds the greatest element in a range and returns an **iterator** to
it, not the value — dereference the result (`*it`) to get the value,
and check the result against `last` first, since an empty range
returns `last`. Compares with `operator<` by default, or a comparator
you supply.

```cpp skip
std::max_element(first, last);                // uses operator<
std::max_element(first, last, comp);           // your ordering
std::max_element(policy, first, last);         // parallel        (since C++17)
std::max_element(policy, first, last, comp);   // parallel + comp (since C++17)
```

Since C++17 the non-policy forms are `constexpr`.

### What you provide

- **first, last** — forward iterators (works on `std::list`, unlike
  algorithms that need random access).
- **comp** — a callable answering *does `a` belong before `b`?*, same
  strict-weak-ordering rules as `std::sort`'s comparator: use
  `<`-style logic, never `<=`, and don't modify the arguments.
- **policy** — an execution policy (C++17) for parallel execution.

### Guarantees and costs

- Exactly `max(N-1, 0)` comparisons, where `N = std::distance(first,
  last)`.
- Returns `last` if the range is empty — dereferencing that is UB, so
  check before using the result.
- If several elements tie for greatest, returns the iterator to the
  **first** one.
- Parallel overloads: if the comparator throws, `std::terminate` is
  called; allocation failure throws `std::bad_alloc`.

### Gotchas

- The return is an iterator, not the value — a common bug treats it as
  the max value directly instead of writing `*result`.
- Don't confuse this with `std::max`, which compares two values (or an
  initializer list) directly and returns a value, not an iterator.
- Empty-range handling was unspecified before C++11 (LWG 212); modern
  implementations always return `last`.

### Example

```cpp
#include <algorithm>
#include <cmath>
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> v{3, 1, -14, 1, 5, 9};

    auto result = std::max_element(v.begin(), v.end());
    std::cout << "max element found at index "
              << std::distance(v.begin(), result)
              << " has value " << *result << '\n';

    result = std::max_element(v.begin(), v.end(), [](int a, int b)
    {
        return std::abs(a) < std::abs(b);
    });
    std::cout << "absolute max element found at index "
              << std::distance(v.begin(), result)
              << " has value " << *result << '\n';
}
```

```text
max element found at index 5 has value 9
absolute max element found at index 2 has value -14
```

### Reference

Full declarations:

```cpp skip
template< class ForwardIt >
ForwardIt max_element( ForwardIt first, ForwardIt last );  // (until C++17)
template< class ForwardIt >
constexpr ForwardIt max_element( ForwardIt first, ForwardIt last );  // (since C++17)
template< class ExecutionPolicy, class ForwardIt >
ForwardIt max_element( ExecutionPolicy&& policy,
                       ForwardIt first, ForwardIt last );  // (2) (since C++17)
template< class ForwardIt, class Compare >
ForwardIt max_element( ForwardIt first, ForwardIt last, Compare comp );  // (until C++17)
template< class ForwardIt, class Compare >
constexpr ForwardIt max_element( ForwardIt first, ForwardIt last,
                                 Compare comp );  // (since C++17)
template< class ExecutionPolicy, class ForwardIt, class Compare >
ForwardIt max_element( ExecutionPolicy&& policy,
                       ForwardIt first, ForwardIt last, Compare comp );  // (4) (since C++17)
```

`ForwardIt` must meet LegacyForwardIterator. `comp` must not modify
the objects passed to it and must accept all value-category
combinations of the dereferenced iterator's type. LWG 212 (applied
retroactively to C++98): the return value for an empty range was
originally unspecified; now guaranteed to be `last`.

### See also

- **min_element** — returns the smallest element in a range
- **minmax_element** (C++11) — smallest and largest in one pass
- **max** — greater of two values (or an initializer list), not a range
- **ranges::max_element** (C++20) — constrained version with projections

---
*Source: https://en.cppreference.com/w/cpp/algorithm/max_element*
