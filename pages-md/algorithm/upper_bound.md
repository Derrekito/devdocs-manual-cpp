# std::upper_bound

Finds the first element in a partitioned (typically sorted) range that
compares strictly greater than `value` — the "just past all matches" edge.
Pair it with `std::lower_bound` for the "at or after" edge, or use
`std::equal_range` to get both at once.

```cpp skip
std::upper_bound(first, last, value);        // uses operator<
std::upper_bound(first, last, value, comp);   // your ordering
```

Since C++20 both overloads are `constexpr`.

### What you provide

- **first, last** — forward iterators over the range to search.
- **value** — the value to compare elements against.
- **comp** — a callable answering *does `a` belong before `b`?*, taking two
  arguments. It need not be a full ordering (`Compare`) — heterogeneous
  comparisons against `value` are fine as long as the range stays
  partitioned with respect to it.

### Guarantees and costs

- The range must be partitioned with respect to `!(value < element)` (or
  `!comp(value, element)`) — all elements where that expression is `true`
  must precede all where it's `false`. A fully-sorted range always
  satisfies this.
- Returns the first element for which `value < element` (or
  `comp(value, element)`) holds, or `last` if none does.
- At most `log2(last - first) + O(1)` comparisons — but for iterators that
  aren't random-access, the number of iterator *increments* is still
  linear, so the log-N comparison count doesn't buy log-N wall-clock time.
  `std::map`, `std::multimap`, `std::set`, and `std::multiset` provide
  their own `upper_bound` member — prefer it over this free function on
  those containers.
- Defect reports corrected the original wording: LWG 270 relaxed the
  ordering requirement to the partitioning above (permitting heterogeneous
  comparisons); LWG 384 corrected the comparison-count bound to
  `log2(...) + O(1)`; LWG 577 confirmed that returning `last` is allowed.

### Gotchas

- An unsorted (or unpartitioned) range doesn't throw or assert — it just
  gives you a wrong iterator silently.
- "Greater than" is strict: an element equal to `value` is *not* included —
  `upper_bound` lands just past the last match, not on it.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

struct PriceInfo { double price; };

int main()
{
    const std::vector<int> data{1, 2, 4, 5, 5, 6};

    auto upper = std::upper_bound(data.begin(), data.end(), 4);
    std::cout << "first > 4: " << *upper
               << " at index " << std::distance(data.begin(), upper) << '\n';

    const std::vector<PriceInfo>
        prices{{100.0}, {101.5}, {102.5}, {102.5}, {107.3}};
    auto p = std::upper_bound(prices.begin(), prices.end(), 102.5,
        [](double value, const PriceInfo& info) { return value < info.price; });
    std::cout << "first price > 102.5: " << p->price << '\n';
}
```

```text
first > 4: 5 at index 3
first price > 102.5: 107.3
```

### Reference

Full declarations:

```cpp skip
template< class ForwardIt, class T >
ForwardIt upper_bound( ForwardIt first, ForwardIt last, const T& value );  // (until C++20)
template< class ForwardIt, class T >
constexpr ForwardIt upper_bound( ForwardIt first, ForwardIt last,
                                 const T& value );  // (since C++20)
template< class ForwardIt, class T, class Compare >
ForwardIt upper_bound( ForwardIt first, ForwardIt last,
                       const T& value, Compare comp );  // (until C++20)
template< class ForwardIt, class T, class Compare >
constexpr ForwardIt upper_bound( ForwardIt first, ForwardIt last,
                                 const T& value, Compare comp );  // (since C++20)
```

`ForwardIt` must satisfy LegacyForwardIterator; `Compare` need only satisfy
BinaryPredicate, not the full Compare requirements.

### See also

- **lower_bound** — first element not less than a given value
- **equal_range** — the full range of elements equivalent to a value
- **partition_point** — the boundary of an already-partitioned range (C++11)
- **ranges::upper_bound** — constrained version with projections (C++20)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/upper_bound*
