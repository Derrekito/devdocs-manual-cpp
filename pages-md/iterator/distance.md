# std::distance

Counts the hops from `first` to `last`. On a LegacyRandomAccessIterator
(`vector`, `deque`, `array`, `string`) this is O(1) — a single
subtraction. On anything else (`list`, `forward_list`, input/output
iterators) it walks the range one element at a time with `++`, so it's
O(N) — the same cost as writing the loop yourself.

```cpp skip
std::distance(first, last)  // hops from first to last; O(1) random-access, else O(N)
```

### What you provide

- **first** — iterator to the first element.
- **last** — iterator marking the end of the range (or, for
  random-access iterators, any second point to measure against).

### Guarantees and costs

- Linear in general — walks `first` to `last` incrementing. Constant
  if `InputIt` additionally meets LegacyRandomAccessIterator (computed
  as `last - first`).
- Since C++11, the result may be negative — only possible with
  random-access iterators, when `first` is reachable from `last`
  (i.e. `last` comes before `first`).
- `constexpr` since C++17.

### Gotchas

- For anything short of random-access (`list`, `forward_list`,
  `istream_iterator`, ...), `last` must be reachable from `first` by
  repeated `++`, or the call is undefined behavior — there is no way
  to detect an unreachable `last` safely.
- `std::distance(v.end(), v.begin())` on a non-random-access container
  is UB; on a random-access one it is well-defined and returns a
  negative count (e.g. `-3`) — this reachable-either-way case was
  clarified by LWG 940.
- Even to just check emptiness, `distance` walks the whole range on
  non-random-access iterators; comparing `first != last` is enough and
  doesn't pay that cost.

### Example

```cpp
#include <iostream>
#include <iterator>
#include <vector>

int main()
{
    std::vector<int> v{3, 1, 4};
    std::cout << "distance(first, last) = "
              << std::distance(v.begin(), v.end()) << '\n'
              << "distance(last, first) = "
              << std::distance(v.end(), v.begin()) << '\n';

    static constexpr auto il = {3, 1, 4};
    // Since C++17, distance can be used in a constexpr context.
    static_assert(std::distance(il.begin(), il.end()) == 3);
    static_assert(std::distance(il.end(), il.begin()) == -3);
}
```

```text
distance(first, last) = 3
distance(last, first) = -3
```

### Reference

```cpp skip
template< class InputIt >
typename std::iterator_traits<InputIt>::difference_type
    distance( InputIt first, InputIt last );  // (constexpr since C++17)
```

Random-access iterators compute `last - first` directly; all other
LegacyInputIterators count via repeated `++first`. `InputIt` must meet
LegacyInputIterator at minimum; the operation is only efficient (O(1))
when it additionally meets LegacyRandomAccessIterator.

### See also

- **advance** — moves an iterator by a given distance, in place
- **count**, **count_if** — count elements matching a criterion
- **ranges::distance** — constrained version; works with a sentinel
  (C++20)

---
*Source: https://en.cppreference.com/w/cpp/iterator/distance*
