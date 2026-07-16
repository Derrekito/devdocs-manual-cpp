# std::all_of, std::any_of, std::none_of

Answer a yes/no question about a whole range with a predicate,
short-circuiting at the first decisive element. Available since C++11.

```cpp skip
std::all_of(first, last, pred);           // true if pred holds for every elem.
std::any_of(first, last, pred);           // true if pred holds for at least one
std::none_of(first, last, pred);          // true if pred holds for none
std::all_of(policy, first, last, pred);   // parallel  (since C++17)
std::any_of(policy, first, last, pred);   // parallel  (since C++17)
std::none_of(policy, first, last, pred);  // parallel  (since C++17)
```

Since C++20 the non-policy forms are `constexpr`.

### What you provide

- **first, last** — input iterators bounding the range to examine
  (forward iterators for the policy overloads).
- **pred** — a unary predicate: takes one element, returns something
  convertible to `bool`, and must not modify it.
- **policy** — an execution policy such as `std::execution::par`
  (C++17) to run in parallel.

### Guarantees and costs

- At most `last - first` applications of the predicate; evaluation
  stops as soon as the answer is decided (`any_of` on the first match,
  `all_of`/`none_of` on the first counterexample).
- On an **empty range**: `all_of` and `none_of` return `true`,
  `any_of` returns `false` (the vacuous-truth convention).
- Parallel overloads: a throwing predicate calls `std::terminate`;
  allocation failure throws `std::bad_alloc`.

### Gotchas

- The empty-range results surprise people who expect `any_of` to also
  default to `true` — check that vacuous truth is the answer you want
  before relying on it for empty input.
- These return a `bool`, not the matching element — use `std::find_if`
  to get the element itself, `std::count_if` to count matches.
- `all_of(r, pred)` is logically `none_of(r, !pred)`; pick whichever
  reads clearest rather than double-negating a predicate.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> v{2, 4, 6, 8};

    bool all_even = std::all_of(v.begin(), v.end(),
                                 [](int x) { return x % 2 == 0; });
    bool any_big  = std::any_of(v.begin(), v.end(),
                                 [](int x) { return x > 5; });
    bool none_neg = std::none_of(v.begin(), v.end(),
                                  [](int x) { return x < 0; });

    std::cout << std::boolalpha << all_even << ' ' << any_big << ' '
              << none_neg << '\n';
}
```

```text
true true true
```

### Reference

Full declarations:

```cpp skip
template< class InputIt, class UnaryPredicate >
bool all_of( InputIt first, InputIt last,
            UnaryPredicate p );  // (since C++11) (until C++20)
template< class InputIt, class UnaryPredicate >
constexpr bool all_of( InputIt first, InputIt last,
                       UnaryPredicate p );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class UnaryPredicate >
bool all_of( ExecutionPolicy&& policy, ForwardIt first, ForwardIt last,
             UnaryPredicate p );  // (2) (since C++17)
template< class InputIt, class UnaryPredicate >
bool any_of( InputIt first, InputIt last,
            UnaryPredicate p );  // (since C++11) (until C++20)
template< class InputIt, class UnaryPredicate >
constexpr bool any_of( InputIt first, InputIt last,
                       UnaryPredicate p );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class UnaryPredicate >
bool any_of( ExecutionPolicy&& policy, ForwardIt first, ForwardIt last,
             UnaryPredicate p );  // (4) (since C++17)
template< class InputIt, class UnaryPredicate >
bool none_of( InputIt first, InputIt last,
             UnaryPredicate p );  // (since C++11) (until C++20)
template< class InputIt, class UnaryPredicate >
constexpr bool none_of( InputIt first, InputIt last,
                        UnaryPredicate p );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class UnaryPredicate >
bool none_of( ExecutionPolicy&& policy, ForwardIt first, ForwardIt last,
              UnaryPredicate p );  // (6) (since C++17)
```

`InputIt` must meet LegacyInputIterator; the policy overloads require
LegacyForwardIterator. `UnaryPredicate` must meet the Predicate
requirements, and its parameter must accept the iterator's value type
by value or const reference (a plain non-const reference parameter is
not allowed).

### See also

- **ranges::all_of, ranges::any_of, ranges::none_of (C++20)** —
  constrained versions with projections

---
*Source: https://en.cppreference.com/w/cpp/algorithm/all_any_none_of*
