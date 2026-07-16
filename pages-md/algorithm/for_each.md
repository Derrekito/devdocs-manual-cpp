# std::for_each

Applies a callable to every element of a range, in order, for its side
effects. A plain range-for is usually clearer for a whole container;
reach for `for_each` when you already have a callable in hand, when
you're operating on a sub-range, or when you want the guaranteed
in-order application that `std::transform` doesn't promise.

```cpp skip
std::for_each(first, last, f);           // apply f to each element, in order
std::for_each(policy, first, last, f);   // parallel, order not guaranteed (since C++17)
```

Since C++20 the non-policy form is `constexpr`.

### What you provide

- **first, last** — input iterators bounding the range (forward
  iterators for the parallel overload).
- **f** — a callable taking one element. If the iterator is mutable
  (e.g. `v.begin()`, not `v.cbegin()`), taking the parameter by
  reference lets `f` modify elements in place. Any return value from
  `f` is ignored.
- **policy** — an execution policy such as `std::execution::par`
  (C++17) to run `f` concurrently — note this drops the ordering
  guarantee the sequential form gives you.

### Guarantees and costs

- Exactly `std::distance(first, last)` applications of `f`.
- The sequential overload applies `f` strictly in order; the parallel
  overload does not.
- Returns `f` itself (moved, since C++11), so a stateful function
  object's accumulated state is still available after the call.
- Parallel overload: if `f` throws, `std::terminate` is called;
  allocation failure throws `std::bad_alloc`. Unlike other parallel
  algorithms, `for_each` is not permitted to copy elements even when
  they're trivially copyable.

### Gotchas

- For the whole container, a range-for (`for (int x : v)`) is more
  readable — save `for_each` for sub-ranges or when passing an
  existing functor.
- If you're accumulating a value (a sum, a count), `std::accumulate`
  (`<numeric>`) says that intent directly; a `for_each` with a
  captured accumulator works but reads as a workaround.
- The mutating form requires a mutable iterator and a by-reference
  parameter (`[](int& x){ ... }`) — a by-value parameter silently
  modifies a copy and leaves the container unchanged.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> v{1, 2, 3, 4};

    std::for_each(v.begin(), v.end(), [](int& x) { x *= 10; });

    std::for_each(v.cbegin(), v.cend(),
                 [](int x) { std::cout << x << ' '; });
    std::cout << '\n';
}
```

```text
10 20 30 40
```

### Reference

Full declarations:

```cpp skip
template< class InputIt, class UnaryFunction >
UnaryFunction for_each( InputIt first, InputIt last, UnaryFunction f );  // (until C++20)
template< class InputIt, class UnaryFunction >
constexpr UnaryFunction for_each( InputIt first, InputIt last,
                                  UnaryFunction f );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class UnaryFunction2 >
void for_each( ExecutionPolicy&& policy,
               ForwardIt first, ForwardIt last, UnaryFunction2 f );  // (since C++17)
```

`InputIt` must meet LegacyInputIterator; the policy overload's
`ForwardIt` must meet LegacyForwardIterator. `UnaryFunction` need only
be MoveConstructible (not CopyConstructible); the policy overload's
`UnaryFunction2` must be CopyConstructible. Return value: the
sequential form returns `f` (`std::move(f)` since C++11); the parallel
form returns nothing. A defect report (LWG 475, applied to C++98)
clarified that `f` is allowed to modify elements when the iterator
type is mutable — despite `for_each` being classified among the
"non-modifying sequence operations."

### See also

- **transform** — applies a function to a range, storing results in a
  destination range
- **for_each_n** — applies a function to the first N elements instead
  of a first/last pair (C++17)
- **ranges::for_each, ranges::for_each_n** — constrained versions with
  projections (C++20)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/for_each*
