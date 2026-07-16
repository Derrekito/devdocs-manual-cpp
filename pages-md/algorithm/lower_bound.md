# std::lower_bound

Binary-searches a partitioned (typically: sorted) range for the first
element that is not less than `value`, returning `last` if every
element is smaller. It's the position where `value` could be inserted
without breaking the ordering — the standard building block for "does
this sorted range contain X, and if not where would it go?"

```cpp skip
std::lower_bound(first, last, value);         // uses operator<
std::lower_bound(first, last, value, comp);   // your ordering
```

Since C++20 both forms are `constexpr`.

### What you provide

- **first, last** — forward iterators bounding a range that is
  **partitioned** with respect to `element < value` (or
  `comp(element, value)`): every element for which that's `true` must
  come before every element for which it's `false`. A fully sorted
  range always satisfies this; so does, e.g., a rotated sorted range
  around the search point — it doesn't have to be sorted everywhere.
- **value** — the value to search for.
- **comp** — a callable answering *does this element belong before
  value?*, taking an element and `value` (or convertible types) and
  returning `bool`. It must not modify its arguments. If you sorted
  with a custom comparator, pass that same comparator here.

### Guarantees and costs

- Logarithmic in `std::distance(first, last)`: at most
  `log2(last - first) + O(1)` comparisons.
- That comparison count is logarithmic **regardless of iterator
  category**, but the number of iterator increments is linear for
  non-random-access iterators — so on a `std::list`, `std::set`, or
  `std::map`, this still costs O(n) wall-clock time even though it
  only compares O(log n) times. Those containers' own member
  `lower_bound()` is the real O(log n) — use it instead of the free
  function.
- Returns an iterator to the first element not less than `value`, or
  `last` if none qualifies.

### Gotchas

- The range **must already be partitioned** the same way `comp`
  partitions it. On unsorted input the result is meaningless, not an
  error — there's no check.
- It can return `last`; dereferencing without checking is undefined
  behavior.
- `std::lower_bound` on a `std::set`/`std::map` compiles (their
  iterators are bidirectional, which is enough to satisfy
  `ForwardIt`), but it's O(n), not O(log n) — see Guarantees above.
  Call `.lower_bound()` on the container instead.
- `lower_bound` finds "not less than"; for "strictly greater than" use
  `std::upper_bound`. Together they bracket a run of equal elements —
  or get both at once from `std::equal_range`.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> v{1, 3, 5, 7, 9};

    auto it = std::lower_bound(v.begin(), v.end(), 5);
    std::cout << "index " << (it - v.begin()) << '\n';

    v.insert(std::lower_bound(v.begin(), v.end(), 6), 6);  // keeps v sorted
    std::cout << v[3] << '\n';
}
```

```text
index 2
6
```

### Reference

Full declarations:

```cpp skip
template< class ForwardIt, class T >
ForwardIt lower_bound( ForwardIt first, ForwardIt last, const T& value );  // (until C++20)
template< class ForwardIt, class T >
constexpr ForwardIt lower_bound( ForwardIt first, ForwardIt last,
                                 const T& value );  // (since C++20)
template< class ForwardIt, class T, class Compare >
ForwardIt lower_bound( ForwardIt first, ForwardIt last,
                       const T& value, Compare comp );  // (until C++20)
template< class ForwardIt, class T, class Compare >
constexpr ForwardIt lower_bound( ForwardIt first, ForwardIt last,
                                 const T& value, Compare comp );  // (since C++20)
```

`ForwardIt` must meet LegacyForwardIterator. `Compare` must meet the
BinaryPredicate requirements but, notably, is **not** required to
satisfy the stricter Compare (strict weak ordering) requirements —
only a partitioning of the range is required, and heterogeneous
comparisons (comparing element and value types that differ) are
permitted. Two defect reports apply: LWG 270 (C++98) relaxed the
original requirement that `Compare` be a strict weak ordering and `T`
be LessThanComparable down to "range is partitioned"; LWG 384 (C++98)
corrected the stated comparison bound from `log(last - first) + 1` to
`log2(last - first) + O(1)`.

### See also

- **upper_bound** — first element *strictly greater* than a value
- **equal_range** — both bounds at once, for a run of equal elements
- **binary_search** — just a bool: is the value present
- **partition_point** — locates the partition point of an
  arbitrarily-partitioned range (C++11)
- **ranges::lower_bound** — constrained version with projections
  (C++20)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/lower_bound*
