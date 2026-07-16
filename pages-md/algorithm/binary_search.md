# std::binary_search

Answers a yes/no question — *does an element equivalent to `value` exist in
this range?* — in O(log N), but only if the range is already sorted (or at
least partitioned) with respect to `value`. If you need *where* it is, use
`std::lower_bound`, `std::upper_bound`, or `std::equal_range` instead.

```cpp skip
std::binary_search(first, last, value);        // uses operator<
std::binary_search(first, last, value, comp);   // your ordering
```

Since C++20 both overloads are `constexpr`.

### What you provide

- **first, last** — forward iterators (works on `std::list` too, just not
  in true O(log N) time — see below).
- **value** — the value to look for.
- **comp** — a callable answering *does `a` belong before `b`?*, taking two
  arguments. It need not be a full ordering (`Compare`) — heterogeneous
  comparisons against `value` are fine as long as the range stays
  partitioned with respect to it.

### Guarantees and costs

- The range must be partitioned with respect to `element < value` (or
  `comp(element, value)`) and with respect to `!(value < element)` (or
  `!comp(value, element)`); a fully-sorted range always satisfies this. An
  unpartitioned range gives no guarantees about the answer.
- At most `log2(last - first) + O(1)` comparisons. On iterators that aren't
  random-access, the number of iterator *increments* is still linear, so
  the log-N comparison count doesn't translate into log-N wall-clock time
  on something like `std::list`.
- A defect report (LWG 270) relaxed the original requirement — early
  wording demanded a strict weak ordering and `T` be LessThanComparable;
  the corrected rule only requires the partitioning above, permitting
  heterogeneous comparisons. Another (LWG 787) corrected the stated
  comparison-count bound to the `log2(...) + O(1)` above.

### Gotchas

- An unsorted (or unpartitioned) range doesn't throw or assert — it just
  gives you a wrong `true`/`false` silently.
- `binary_search` only tells you whether a match exists, not where — for
  the position, or the full range of equal elements, use `lower_bound`,
  `upper_bound`, or `equal_range`.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> haystack{1, 3, 4, 5, 9};
    std::vector<int> needles{1, 2, 3};

    for (const auto needle : needles)
    {
        std::cout << "Searching for " << needle << '\n';
        if (std::binary_search(haystack.begin(), haystack.end(), needle))
            std::cout << "Found " << needle << '\n';
        else
            std::cout << "No dice!\n";
    }
}
```

```text
Searching for 1
Found 1
Searching for 2
No dice!
Searching for 3
Found 3
```

### Reference

Full declarations:

```cpp skip
template< class ForwardIt, class T >
bool binary_search( ForwardIt first, ForwardIt last, const T& value );  // (until C++20)
template< class ForwardIt, class T >
constexpr bool binary_search( ForwardIt first, ForwardIt last,
                              const T& value );  // (since C++20)
template< class ForwardIt, class T, class Compare >
bool binary_search( ForwardIt first, ForwardIt last,
                    const T& value, Compare comp );  // (until C++20)
template< class ForwardIt, class T, class Compare >
constexpr bool binary_search( ForwardIt first, ForwardIt last,
                              const T& value, Compare comp );  // (since C++20)
```

`ForwardIt` must satisfy LegacyForwardIterator; `Compare` need only satisfy
BinaryPredicate, not the full Compare requirements. It's implemented in
terms of `lower_bound`: `first = std::lower_bound(first, last, value);
return first != last && !(value < *first);`.

### See also

- **equal_range** — the full range of elements equivalent to a value
- **lower_bound** — first element not less than a given value
- **upper_bound** — first element greater than a given value
- **ranges::binary_search** — constrained version with projections (C++20)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/binary_search*
