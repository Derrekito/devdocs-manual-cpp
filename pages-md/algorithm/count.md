# std::count, std::count_if

Tallies how many elements in a range match a condition: `count` counts
elements equal to a value, `count_if` counts elements satisfying a
predicate. Always O(n) — it scans the whole range regardless of how
many matches it finds.

```cpp skip
std::count(first, last, value);           // # elements == value
std::count_if(first, last, pred);         // # elements pred() accepts
std::count(policy, first, last, value);   // parallel        (since C++17)
std::count_if(policy, first, last, pred); // parallel        (since C++17)
```

Since C++20 the non-policy forms are `constexpr`.

### What you provide

- **first, last** — input iterators bounding the range to scan
  (forward iterators for the parallel overloads).
- **value** — compared to each element with `operator==`.
- **pred** — a callable taking one element (by value or const
  reference) and returning something convertible to `bool`. It must
  not modify the element it's given.
- **policy** — an execution policy such as `std::execution::par`
  (C++17) to count with multiple threads.

### Guarantees and costs

- Exactly N comparisons/predicate applications, where N is
  `std::distance(first, last)` — every element is visited, always.
- Returns `iterator_traits<InputIt>::difference_type` — a signed
  integer type (typically `long` or `long long`), not `int`.
- Parallel overloads: if the predicate throws, `std::terminate` is
  called; allocation failure throws `std::bad_alloc`.

### Gotchas

- Capture the result with `auto`; treating it as `int` invites
  `-Wsign-compare` warnings or narrowing when the range is large.
- It's O(n) even when the range is sorted or the container is a
  `std::set`/`std::map` — those have their own `.count()` member in
  O(log n); prefer it. For a sorted range, `std::equal_range` gives the
  matching span in O(log n).
- If you only need to know whether *any* element matches, `count_if`
  still scans everything — `std::any_of` (or `std::find` compared
  against `end()`) stops at the first match instead.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> v{1, 2, 2, 3, 2, 4};

    auto twos = std::count(v.begin(), v.end(), 2);
    auto evens = std::count_if(v.begin(), v.end(),
                               [](int x) { return x % 2 == 0; });

    std::cout << "2s: " << twos << ", evens: " << evens << '\n';
}
```

```text
2s: 3, evens: 4
```

### Reference

Full declarations:

```cpp skip
template< class InputIt, class T >
typename iterator_traits<InputIt>::difference_type
    count( InputIt first, InputIt last, const T& value );  // (until C++20)
template< class InputIt, class T >
constexpr typename iterator_traits<InputIt>::difference_type
    count( InputIt first, InputIt last, const T& value );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class T >
typename iterator_traits<ForwardIt>::difference_type
    count( ExecutionPolicy&& policy,
           ForwardIt first, ForwardIt last, const T& value );  // (since C++17)
template< class InputIt, class UnaryPredicate >
typename iterator_traits<InputIt>::difference_type
    count_if( InputIt first, InputIt last, UnaryPredicate p );  // (until C++20)
template< class InputIt, class UnaryPredicate >
constexpr typename iterator_traits<InputIt>::difference_type
    count_if( InputIt first, InputIt last, UnaryPredicate p );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class UnaryPredicate >
typename iterator_traits<ForwardIt>::difference_type
    count_if( ExecutionPolicy&& policy,
              ForwardIt first, ForwardIt last, UnaryPredicate p );  // (since C++17)
```

`InputIt` must meet LegacyInputIterator; the policy overloads'
`ForwardIt` must meet LegacyForwardIterator. The predicate's parameter
must accept the value type by const reference or value, regardless of
value category, and must not modify it. Return value: the number of
iterators `it` for which `*it == value` (count) or `p(*it) != false`
(count_if) holds. For the size of the range with no filtering
criterion, use `std::distance` instead. A defect report (LWG 283,
applied to C++98) removed the requirement that `T` be
EqualityComparable, since the value type of `InputIt` need not be `T`.

### See also

- **distance** — returns the distance between two iterators, with no
  filtering
- **ranges::count, ranges::count_if** — constrained versions with
  projections (C++20)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/count*
