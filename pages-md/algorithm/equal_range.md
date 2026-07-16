# std::equal_range

Returns the sub-range of elements equivalent to `value` — the pair of
iterators you'd get from calling both `std::lower_bound` and
`std::upper_bound`, computed together. Use it when you need the whole run
of matches (e.g. duplicates in a sorted `vector`), not just yes/no
(`std::binary_search`) or one edge (`lower_bound`/`upper_bound`).

```cpp skip
std::equal_range(first, last, value);        // uses operator<
std::equal_range(first, last, value, comp);   // your ordering
```

Since C++20 both overloads are `constexpr`.

### What you provide

- **first, last** — forward iterators over the range to search.
- **value** — the value to match.
- **comp** — a callable answering *does `a` belong before `b`?*, taking two
  arguments. It need not be a full ordering (`Compare`) — heterogeneous
  comparisons against `value` are fine as long as the range stays
  partitioned with respect to it.

### Guarantees and costs

- The range must be partitioned with respect to `element < value` (or
  `comp(element, value)`) and with respect to `!(value < element)` (or
  `!comp(value, element)`); a fully-sorted range always satisfies this.
- Returns a `std::pair` of iterators: the first element not less than
  `value`, and the first element greater than `value`. If no such elements
  exist, `last` is returned for that end.
- At most `2 · log2(last - first) + O(1)` comparisons. On iterators that
  aren't random-access, the number of iterator increments is still linear.
  `std::set` and `std::multiset` provide their own `equal_range` member —
  prefer it over this free function on those containers.
- A defect report (LWG 270) relaxed the original requirement from a strict
  weak ordering on `T` to the partitioning stated above, permitting
  heterogeneous comparisons. Another (LWG 384) corrected the stated
  comparison-count bound to the `2 · log2(...) + O(1)` above (the original
  bound was unimplementable — a single-element range needs 2 comparisons
  but the old bound allowed only 1).

### Gotchas

- An unsorted (or unpartitioned) range doesn't throw or assert — it just
  gives you a wrong (possibly empty) range silently.
- The two returned iterators may be equal — that means no matching elements
  exist, not that something went wrong.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

struct S
{
    int number;
    char name;
    // name is ignored by this comparison operator
    bool operator<(const S& s) const { return number < s.number; }
};

struct Comp
{
    bool operator()(const S& s, int i) const { return s.number < i; }
    bool operator()(int i, const S& s) const { return i < s.number; }
};

template<class It>
void print(It first, It last)
{
    for (bool sep = false; first != last; ++first, sep = true)
        std::cout << (sep ? " " : "") << first->name;
    std::cout << '\n';
}

int main()
{
    // not fully ordered, only partitioned w.r.t. S::operator<
    const std::vector<S> vec{{1, 'A'}, {2, 'B'}, {2, 'C'},
                             {2, 'D'}, {4, 'G'}, {3, 'F'}};
    const S value{2, '?'};

    const auto p = std::equal_range(vec.begin(), vec.end(), value);
    print(p.first, p.second);

    const auto p2 = std::equal_range(vec.begin(), vec.end(), 2, Comp{});
    print(p2.first, p2.second);
}
```

```text
B C D
B C D
```

### Reference

Full declarations:

```cpp skip
template< class ForwardIt, class T >
std::pair<ForwardIt, ForwardIt>
    equal_range( ForwardIt first, ForwardIt last, const T& value );  // (until C++20)
template< class ForwardIt, class T >
constexpr std::pair<ForwardIt, ForwardIt>
    equal_range( ForwardIt first, ForwardIt last, const T& value );  // (since C++20)
template< class ForwardIt, class T, class Compare >
std::pair<ForwardIt, ForwardIt>
    equal_range( ForwardIt first, ForwardIt last,
                 const T& value, Compare comp );  // (until C++20)
template< class ForwardIt, class T, class Compare >
constexpr std::pair<ForwardIt, ForwardIt>
    equal_range( ForwardIt first, ForwardIt last,
                 const T& value, Compare comp );  // (since C++20)
```

`ForwardIt` must satisfy LegacyForwardIterator; `Compare` need only satisfy
BinaryPredicate, not the full Compare requirements. It's implemented as
`std::make_pair(std::lower_bound(first, last, value),
std::upper_bound(first, last, value))` (with `comp` threaded through both
calls in the comparator overload).

### See also

- **lower_bound** — first element not less than a given value
- **upper_bound** — first element greater than a given value
- **binary_search** — checks whether an equivalent element exists at all
- **ranges::equal_range** — constrained version with projections (C++20)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/equal_range*
