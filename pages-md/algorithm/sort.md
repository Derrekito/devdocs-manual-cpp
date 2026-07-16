# std::sort

Sorts a range in place, ascending by default. It is fast — O(N·log N)
worst case — but **not stable**: elements that compare equal may end up
in any order relative to each other (use `std::stable_sort` when their
original order matters).

```cpp skip
std::sort(first, last);                  // ascending, uses operator<
std::sort(first, last, comp);            // your ordering
std::sort(policy, first, last);          // parallel        (since C++17)
std::sort(policy, first, last, comp);    // parallel + comp (since C++17)
```

Since C++20 the non-policy forms are `constexpr`.

### What you provide

- **first, last** — random-access iterators (a `vector`, `array`,
  `deque`, `std::string`, or C array). `std::list` doesn't qualify —
  use its member `sort()`. `map`/`set` keep themselves ordered and
  cannot be sorted.
- **comp** — a callable answering one question: *does `a` belong
  before `b`?* It gets two elements and returns `bool`. It must be a
  strict weak ordering: use `<`-style logic, never `<=`, and give
  equal elements `false` both ways. It must not modify its arguments.
- **policy** — an execution policy such as `std::execution::par`
  (C++17) to sort with multiple threads. Worth it for large ranges of
  cheap-to-compare elements; measure before committing.

### Guarantees and costs

- O(N·log N) comparisons in the **worst case** (since C++11; C++98
  only required this on average). Implementations typically use
  introsort.
- Elements are moved around, so any pointer, reference, or iterator
  you held "by position" now refers to a different element's value.
- Not stable — see `std::stable_sort`.
- Parallel overloads: if a comparator throws, `std::terminate` is
  called; allocation failure throws `std::bad_alloc`.

### Gotchas

- A comparator that is not a strict weak ordering — returning
  `a <= b`, or inconsistent answers — is **undefined behavior**, and
  in practice crashes in optimized builds rather than merely
  mis-sorting.
- Sorting only needs the range sorted once: if you sort just to find
  the few smallest elements, `std::partial_sort` or
  `std::nth_element` is cheaper.
- After sorting, `std::lower_bound` / `std::binary_search` give
  O(log N) lookups — unsorted ranges silently give wrong answers with
  those.

### Example

```cpp
#include <algorithm>
#include <functional>
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> v{5, 7, 4, 2, 8};

    std::sort(v.begin(), v.end());                       // 2 4 5 7 8
    std::sort(v.begin(), v.end(), std::greater<>{});     // 8 7 5 4 2
    std::sort(v.begin(), v.end(),
              [](int a, int b) { return a < b; });       // 2 4 5 7 8

    for (int x : v)
        std::cout << x << ' ';
    std::cout << '\n';
}
```

```text
2 4 5 7 8
```

### Reference

Full declarations:

```cpp skip
template< class RandomIt >
void sort( RandomIt first, RandomIt last );  // (until C++20)
template< class RandomIt >
constexpr void sort( RandomIt first, RandomIt last );  // (since C++20)
template< class ExecutionPolicy, class RandomIt >
void sort( ExecutionPolicy&& policy,
           RandomIt first, RandomIt last );  // (2) (since C++17)
template< class RandomIt, class Compare >
void sort( RandomIt first, RandomIt last, Compare comp );  // (until C++20)
template< class RandomIt, class Compare >
constexpr void sort( RandomIt first, RandomIt last, Compare comp );  // (since C++20)
template< class ExecutionPolicy, class RandomIt, class Compare >
void sort( ExecutionPolicy&& policy,
           RandomIt first, RandomIt last, Compare comp );  // (4) (since C++17)
```

Formally: the range is sorted with respect to `comp` when for every
element, no later element compares before an earlier one
(`comp(later, earlier)` is `false`). The policy overloads only
participate in overload resolution for genuine execution-policy types.
`RandomIt` must be a LegacyRandomAccessIterator whose value type is
move-constructible and move-assignable; `Compare` must satisfy the
standard Compare requirements (strict weak ordering).

### See also

- **stable_sort** — same, but equal elements keep their order
- **partial_sort** — sort only the first N elements of the range
- **nth_element** — place one element in its sorted position, O(N)
- **is_sorted** — check whether a range is already sorted (C++11)
- **ranges::sort** — constrained version with projections (C++20)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/sort*
