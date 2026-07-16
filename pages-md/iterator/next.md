# std::next

Returns a **copy** of `it` moved `n` steps forward (or `-n` back if
`n` is negative) — `it` itself is left untouched. This is the value-
returning counterpart to **advance**, which mutates its argument in
place and returns nothing. Reach for `next` when you want the
successor as a value — passed to another call, or used mid-expression
— without disturbing the iterator you already have.

```cpp skip
std::next(it)      // copy of it, advanced by 1              // (since C++11)
std::next(it, n)   // copy of it, advanced by n (or -n back)  // (since C++11)
```

### What you provide

- **it** — an iterator meeting LegacyInputIterator. Moving forward
  (`n >= 0`) only needs that; moving backward (`n < 0`) additionally
  requires `it` to meet LegacyBidirectionalIterator, or the behavior
  is undefined.
- **n** — number of elements to move forward; negative moves backward.
  Defaults to 1.

### Guarantees and costs

- Returns a new iterator holding the `n`th successor of `it`; `it`
  itself is not modified.
- Linear in `n` in general; O(1) if `InputIt` additionally meets
  LegacyRandomAccessIterator.
- `constexpr` since C++17.

### Gotchas

- `std::next` does not mutate `it`. If you meant to move a loop
  iterator forward in place, that's `std::advance(it, n)`, not
  `std::next`.
- `++c.begin()` is not guaranteed to compile — `c.begin()` is an
  rvalue, and no LegacyInputIterator requirement guarantees that
  incrementing an rvalue works. `std::next(c.begin())` always works;
  that's part of why it exists.
- A negative `n` needs `it` to be bidirectional-or-better; on a plain
  forward or input iterator that's undefined behavior.

### Example

```cpp
#include <iostream>
#include <iterator>
#include <vector>

int main()
{
    std::vector<int> v{4, 5, 6};

    auto it = v.begin();
    auto nx = std::next(it, 2);
    std::cout << *it << ' ' << *nx << '\n';

    it = v.end();
    nx = std::next(it, -2);
    std::cout << ' ' << *nx << '\n';
}
```

```text
4 6
 5
```

### Reference

```cpp skip
template< class InputIt >
InputIt next( InputIt it, typename std::iterator_traits<InputIt>::difference_type n = 1 );  // (since C++11) (until C++17)
template< class InputIt >
constexpr
InputIt next( InputIt it, typename std::iterator_traits<InputIt>::difference_type n = 1 );  // (since C++17)
```

Equivalent to `std::advance(it, n); return it;`. Originally (C++11)
`next` required LegacyForwardIterator; LWG 2353 relaxed this to
LegacyInputIterator.

### See also

- **prev** — decrement an iterator, returning a copy (C++11)
- **advance** — mutates an iterator in place by a given distance
- **distance** — the distance between two iterators
- **ranges::next** — constrained version; can advance to a bound
  (C++20)

---
*Source: https://en.cppreference.com/w/cpp/iterator/next*
