# std::advance

Moves `it` forward (or backward, for negative `n`) by `n` elements
**in place** and returns nothing. This is the mutating counterpart to
**next**/**prev**, which leave the original iterator untouched and
hand back an advanced copy instead — in fact both are implemented as a
call to `advance` followed by returning the (copied) iterator.

```cpp skip
std::advance(it, n);  // it moves n elements in place, returns void
```

### What you provide

- **it** — iterator to advance, taken by reference and mutated in
  place; must meet LegacyInputIterator.
- **n** — number of elements to move. If negative, `it` is
  decremented instead, which requires `it` to also meet
  LegacyBidirectionalIterator — otherwise the behavior is undefined.

### Guarantees and costs

- No return value; the only effect is mutating `it`.
- Linear in `n` in general; O(1) if `InputIt` additionally meets
  LegacyRandomAccessIterator (implemented as `it += n`).
- `constexpr` since C++17.

### Gotchas

- Advancing past the valid range is undefined behavior: incrementing a
  non-incrementable iterator (such as past-the-end) or decrementing a
  non-decrementable one (such as before-begin) is UB. `advance` does
  not check bounds.
- Because it mutates in place and returns `void`, it can't be chained
  or used inline in an expression — use `std::next`/`std::prev` when
  you need the result as a value.

### Example

```cpp
#include <iostream>
#include <iterator>
#include <vector>

int main()
{
    std::vector<int> v{3, 1, 4};

    auto vi = v.begin();
    std::advance(vi, 2);
    std::cout << *vi << ' ';

    vi = v.end();
    std::advance(vi, -2);
    std::cout << *vi << '\n';
}
```

```text
4 1
```

### Reference

```cpp skip
template< class InputIt, class Distance >
void advance( InputIt& it, Distance n );  // (until C++17)
template< class InputIt, class Distance >
constexpr void advance( InputIt& it, Distance n );  // (since C++17)
```

Dispatches on `it`'s iterator category: random-access iterators use
`it += n` directly (O(1)); bidirectional iterators loop `++it`/`--it`
depending on the sign of `n`; input iterators only loop `++it` (`n`
must be non-negative). See the standard library implementations in
libstdc++ or libc++ for the exact dispatch.

### See also

- **next** — increment an iterator, returning a copy (C++11)
- **prev** — decrement an iterator, returning a copy (C++11)
- **distance** — the distance between two iterators
- **ranges::advance** — constrained version; can advance to a bound
  (C++20)

---
*Source: https://en.cppreference.com/w/cpp/iterator/advance*
