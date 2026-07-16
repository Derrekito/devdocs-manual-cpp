# std::prev

Returns a **copy** of `it` moved `n` steps backward (or `-n` forward
if `n` is negative) — `it` itself is left untouched. This is the
value-returning counterpart to **advance**, which mutates its argument
in place and returns nothing. Reach for `prev` when you want the
predecessor as a value without disturbing the iterator you already
have.

```cpp skip
std::prev(it)      // copy of it, moved back by 1                // (since C++11)
std::prev(it, n)   // copy of it, moved back by n (or forward)    // (since C++11)
```

### What you provide

- **it** — an iterator meeting LegacyBidirectionalIterator. Unlike
  `next`, this is required regardless of the sign of `n`, since moving
  backward is only well-defined for bidirectional iterators.
- **n** — number of elements to move backward; negative moves forward.
  Defaults to 1.

### Guarantees and costs

- Returns a new iterator holding the `n`th predecessor of `it`; `it`
  itself is not modified.
- Linear in `n` in general; O(1) if `BidirIt` additionally meets
  LegacyRandomAccessIterator.
- `constexpr` since C++17.

### Gotchas

- `std::prev` does not mutate `it`. If you meant to move a loop
  iterator backward in place, that's `std::advance(it, -n)`, not
  `std::prev`.
- `--c.end()` is not guaranteed to compile — `c.end()` is an rvalue,
  and no iterator requirement guarantees that decrementing an rvalue
  works. `std::prev(c.end())` always works; that's part of why it
  exists.

### Example

```cpp
#include <iostream>
#include <iterator>
#include <vector>

int main()
{
    std::vector<int> v{3, 1, 4};

    auto it = v.end();
    auto pv = std::prev(it, 2);
    std::cout << *pv << '\n';

    it = v.begin();
    pv = std::prev(it, -2);
    std::cout << *pv << '\n';
}
```

```text
1
4
```

### Reference

```cpp skip
template< class BidirIt >
BidirIt prev( BidirIt it, typename std::iterator_traits<BidirIt>::difference_type n = 1 );  // (since C++11) (until C++17)
template< class BidirIt >
constexpr
BidirIt prev( BidirIt it, typename std::iterator_traits<BidirIt>::difference_type n = 1 );  // (since C++17)
```

Equivalent to `std::advance(it, -n); return it;`.

### See also

- **next** — increment an iterator, returning a copy (C++11)
- **advance** — mutates an iterator in place by a given distance
- **distance** — the distance between two iterators
- **ranges::prev** — constrained version; can move back to a bound
  (C++20)

---
*Source: https://en.cppreference.com/w/cpp/iterator/prev*
