# std::stable_partition

Reorders a range in place so every element for which a predicate returns
`true` comes before every element for which it returns `false`, the same
split as `std::partition` — but preserves each group's original relative
order. That stability costs more: it needs a temporary buffer, or extra
swaps if one can't be allocated.

```cpp skip
std::stable_partition(first, last, pred);           // stable split
std::stable_partition(policy, first, last, pred);   // parallel  (since C++17)
```

`constexpr` since C++26.

### What you provide

- **first, last** — bidirectional iterators (ValueSwappable and
  LegacyBidirectionalIterator); the dereferenced type must be
  MoveConstructible and MoveAssignable. libstdc++ and libc++ also accept
  forward iterators as a non-standard extension.
- **pred** — a unary predicate: return `true` if the element belongs in
  the first group. It must not modify the element, and its parameter type
  must not be a non-const reference (unless move is equivalent to copy
  for that type).
- **policy** — an execution policy (C++17) to partition with multiple
  threads.

### Guarantees and costs

- Returns an iterator to the first element of the "false" group — the
  partition point, same contract as `std::partition`.
- Relative order within each group is preserved — this is the difference
  from `std::partition`.
- With enough extra memory: exactly N applications of `pred` and O(N)
  swaps (N is the distance from `first` to `last`). If the internal
  buffer allocation fails, it falls back to at most N·log(N) swaps
  instead — silently, with no error reported.
- Parallel overload: N·log(N) swaps, O(N) predicate applications; a
  predicate that throws calls `std::terminate` for standard policies, and
  allocation failure throws `std::bad_alloc`.

### Gotchas

- The stability guarantee isn't free — when relative order doesn't
  matter (e.g. quicksort-style repartitioning), plain `std::partition` is
  cheaper.
- A failed internal allocation degrades performance (more swaps) without
  any visible signal — don't assume O(N) swaps under memory pressure.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> v{0, 0, 3, -1, 2, 4, 5, 0, 7};
    std::stable_partition(v.begin(), v.end(), [](int n) { return n > 0; });
    for (int n : v)
        std::cout << n << ' ';
    std::cout << '\n';
}
```

```text
3 2 4 5 7 0 0 -1 0
```

### Reference

Full declarations:

```cpp skip
template< class BidirIt, class UnaryPredicate >
BidirIt stable_partition( BidirIt first, BidirIt last, UnaryPredicate p );  // (1) (constexpr since C++26)
template< class ExecutionPolicy, class BidirIt, class UnaryPredicate >
BidirIt stable_partition( ExecutionPolicy&& policy,
                          BidirIt first, BidirIt last, UnaryPredicate p );  // (2) (since C++17)
```

Feature-test macro `__cpp_lib_constexpr_algorithms` (value `202306L`)
signals the C++26 `constexpr` support.

### See also

- **partition** — same split, unstable, cheaper
- **ranges::stable_partition** (C++20) — constrained version that
  preserves relative order

---
*Source: https://en.cppreference.com/w/cpp/algorithm/stable_partition*
