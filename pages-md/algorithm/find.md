# std::find, std::find_if, std::find_if_not

Scans a range front-to-back for the first element matching a
condition, returning `last` if nothing matches. `find` matches by
equality (`operator==`); `find_if`/`find_if_not` match (or reject) by
predicate. Linear time — for repeated lookups on a sorted range or a
`set`/`map`, prefer binary search or the container's own `find`.

```cpp skip
std::find(first, last, value);           // first element == value
std::find_if(first, last, pred);         // first element pred() accepts
std::find_if_not(first, last, pred);     // first element pred() rejects
std::find(policy, first, last, value);   // parallel        (since C++17)
std::find_if(policy, first, last, pred); // parallel        (since C++17)
```

Since C++20 all three non-policy forms are `constexpr`.

### What you provide

- **first, last** — input iterators bounding the range to search
  (forward iterators for the parallel overloads).
- **value** — compared to each element with `operator==`.
- **pred** — a callable taking one element (by value or const
  reference) and returning something convertible to `bool`. It must
  not modify the element it's given.
- **policy** — an execution policy such as `std::execution::par`
  (C++17) to search with multiple threads.

### Guarantees and costs

- At most N comparisons/predicate applications, where N is
  `std::distance(first, last)` — linear, not binary, search.
- Parallel overloads: if the predicate throws, `std::terminate` is
  called; allocation failure throws `std::bad_alloc`.
- Returns `last` on no match — never a null iterator or an exception
  for "not found."

### Gotchas

- The result is an **iterator**, not a bool or index — always compare
  against `last`/`end()` before dereferencing. Get a position with
  `it - first` only on random-access ranges.
- `find` is O(n) even when the range happens to be sorted; sorted data
  wants `std::lower_bound` or `std::binary_search` (O(log n)).
  `std::set`/`std::map` have their own O(log n) `.find()` member —
  prefer it over the free function.
- For substring search in a `std::string`, use its member `.find()`,
  not `std::find` (which only matches single characters).

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> v{4, 8, 15, 16, 23};

    auto it = std::find(v.begin(), v.end(), 15);
    std::cout << (it != v.end() ? "found\n" : "absent\n");

    auto ev = std::find_if(v.begin(), v.end(),
                           [](int x) { return x % 2 == 0; });
    std::cout << *ev << '\n';
}
```

```text
found
4
```

### Reference

Full declarations:

```cpp skip
template< class InputIt, class T >
InputIt find( InputIt first, InputIt last, const T& value );  // (until C++20)
template< class InputIt, class T >
constexpr InputIt find( InputIt first, InputIt last, const T& value );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class T >
ForwardIt find( ExecutionPolicy&& policy,
                ForwardIt first, ForwardIt last, const T& value );  // (since C++17)
template< class InputIt, class UnaryPredicate >
InputIt find_if( InputIt first, InputIt last, UnaryPredicate p );  // (until C++20)
template< class InputIt, class UnaryPredicate >
constexpr InputIt find_if( InputIt first, InputIt last, UnaryPredicate p );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class UnaryPredicate >
ForwardIt find_if( ExecutionPolicy&& policy,
                   ForwardIt first, ForwardIt last, UnaryPredicate p );  // (since C++17)
template< class InputIt, class UnaryPredicate >
InputIt find_if_not( InputIt first, InputIt last, UnaryPredicate q );  // (since C++11) (until C++20)
template< class InputIt, class UnaryPredicate >
constexpr InputIt find_if_not( InputIt first, InputIt last, UnaryPredicate q );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class UnaryPredicate >
ForwardIt find_if_not( ExecutionPolicy&& policy,
                       ForwardIt first, ForwardIt last, UnaryPredicate q );  // (since C++17)
```

`InputIt` must meet LegacyInputIterator; the policy overloads need
LegacyForwardIterator. `UnaryPredicate` must meet the Predicate
requirements and must not modify the element passed to it — a
parameter type of `VT&` is not allowed. The predicate expression must
accept the value type by const reference or value, regardless of
value category. Return value: the first iterator `it` for which
`*it == value` (find), `p(*it)` (find_if), or `!q(*it)` (find_if_not)
holds, or `last`. Before C++11, `find_if_not` was written as
`find_if` with a negated predicate. A defect report (LWG 283, applied
to C++98) removed the requirement that `T` be EqualityComparable,
since the value type of `InputIt` need not be `T`.

### See also

- **find_first_of** — searches for any one of a set of elements
- **mismatch** — finds the first position where two ranges differ
- **search** — searches for a subrange of elements
- **ranges::find, ranges::find_if, ranges::find_if_not** — constrained
  versions with projections (C++20)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/find*
