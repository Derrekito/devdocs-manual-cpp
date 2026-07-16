# std::merge

Combines two already-sorted ranges into one sorted range in O(N) — reach
for this instead of concatenating and calling `std::sort` again when you
already have two sorted sequences, since re-sorting the concatenation costs
O(N log N) for no benefit.

```cpp skip
std::merge(first1, last1, first2, last2, d_first);              // uses operator<
std::merge(first1, last1, first2, last2, d_first, comp);        // your ordering
std::merge(policy, first1, last1, first2, last2, d_first);      // parallel        (since C++17)
std::merge(policy, first1, last1, first2, last2, d_first, comp);// parallel + comp (since C++17)
```

Since C++20 the non-policy forms are `constexpr`.

### What you provide

- **first1, last1** / **first2, last2** — the two sorted input ranges (any
  input iterators).
- **d_first** — an output iterator for the merged result (e.g.
  `std::back_inserter(dst)`). The destination range must not overlap either
  input range — the two inputs may overlap each other, but the destination
  may not overlap either.
- **comp** — a callable answering *does `a` belong before `b`?*, taking two
  elements. Both inputs must already be sorted with respect to it.
- **policy** — an execution policy such as `std::execution::par` (C++17);
  this overload requires forward iterators, not just input iterators.

### Guarantees and costs

- Stable: for elements that compare equal, all elements from the first
  range precede all elements from the second range, and each range's
  internal relative order is preserved.
- With `N = distance(first1,last1) + distance(first2,last2)`: at most
  `N - 1` comparisons for the non-parallel overloads, `O(N)` for the
  parallel ones.
- Parallel overloads: if a comparator throws, `std::terminate` is called;
  allocation failure throws `std::bad_alloc`.
- Writes exactly `N` elements to the output — contrast with
  `std::set_union`, which collapses duplicates shared between the ranges
  and can write fewer.
- A defect report (LWG 780) filled in a gap in the original C++98 wording,
  which left the merge operation itself undefined; later revisions define
  it as specified above.

### Gotchas

- Both inputs must actually be sorted (with respect to the same `comp` you
  pass in) — unsorted input doesn't throw, it just produces a garbled,
  silently-wrong result.
- The destination range must not alias either source range; violating that
  is undefined behavior.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <vector>

int main()
{
    const std::vector<int> v1{1, 3, 5, 7};
    const std::vector<int> v2{2, 3, 4, 8};

    std::vector<int> dst;
    std::merge(v1.begin(), v1.end(), v2.begin(), v2.end(),
               std::back_inserter(dst));

    for (int x : dst)
        std::cout << x << ' ';
    std::cout << '\n';
}
```

```text
1 2 3 3 4 5 7 8
```

### Reference

Full declarations:

```cpp skip
template< class InputIt1, class InputIt2, class OutputIt >
OutputIt merge( InputIt1 first1, InputIt1 last1,
                InputIt2 first2, InputIt2 last2,
                OutputIt d_first );  // (until C++20)
template< class InputIt1, class InputIt2, class OutputIt >
constexpr OutputIt merge( InputIt1 first1, InputIt1 last1,
                          InputIt2 first2, InputIt2 last2,
                          OutputIt d_first );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2,
          class ForwardIt3 >
ForwardIt3 merge( ExecutionPolicy&& policy,
                  ForwardIt1 first1, ForwardIt1 last1,
                  ForwardIt2 first2, ForwardIt2 last2,
                  ForwardIt3 d_first );  // (2) (since C++17)
template< class InputIt1, class InputIt2, class OutputIt, class Compare >
OutputIt merge( InputIt1 first1, InputIt1 last1,
                InputIt2 first2, InputIt2 last2,
                OutputIt d_first, Compare comp );  // (until C++20)
template< class InputIt1, class InputIt2, class OutputIt, class Compare >
constexpr OutputIt merge( InputIt1 first1, InputIt1 last1,
                          InputIt2 first2, InputIt2 last2,
                          OutputIt d_first, Compare comp );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2,
          class ForwardIt3, class Compare >
ForwardIt3 merge( ExecutionPolicy&& policy,
                  ForwardIt1 first1, ForwardIt1 last1,
                  ForwardIt2 first2, ForwardIt2 last2,
                  ForwardIt3 d_first, Compare comp );  // (4) (since C++17)
```

Returns an output iterator to one past the last element copied. `InputIt1`,
`InputIt2` need LegacyInputIterator; the policy overloads need
LegacyForwardIterator for all three range arguments; `OutputIt` needs
LegacyOutputIterator.

### See also

- **inplace_merge** — merges two sorted ranges that sit next to each other
- **set_union** — like merge, but collapses duplicates shared by both ranges
- **is_sorted** — checks whether a range is already sorted (C++11)
- **ranges::merge** — constrained version with projections (C++20)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/merge*
