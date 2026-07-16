# std::replace, std::replace_if

Overwrites, in place, every element equal to `old_value` (or matching
a predicate) with `new_value`. Unlike `remove`, this never touches the
container's size — it's a straight element-by-element rewrite, so no
`erase` step is needed afterward.

```cpp skip
std::replace(first, last, old_value, new_value);         // == old_value
std::replace_if(first, last, pred, new_value);            // matching pred
std::replace(policy, first, last, old_value, new_value);  // parallel        (since C++17)
std::replace_if(policy, first, last, pred, new_value);    // parallel + pred (since C++17)
```

`constexpr` since C++20.

### What you provide

- **first, last** — forward iterators bounding the range.
- **old_value** — elements equal to this (via `operator==`) are
  overwritten.
- **pred** — `replace_if` only: a callable returning `true` for
  elements to overwrite; must not modify what it's given.
- **new_value** — the replacement, assigned via `*it = new_value`.
- **policy** — an execution policy (C++17) for parallel execution.

### Guarantees and costs

- `replace`: exactly `N` comparisons with `old_value`, where
  `N = std::distance(first, last)`.
- `replace_if`: exactly `N` predicate applications.
- No return value; range size and element order are unchanged.
- Parallel overloads: if a predicate/assignment throws,
  `std::terminate` is called; allocation failure throws
  `std::bad_alloc`.

### Gotchas

- Both `old_value` and `new_value` are taken by reference: passing a
  reference to an element inside `[first, last)` can misbehave once
  that element gets overwritten mid-pass.
- `*first = new_value` must be valid (until C++20; since C++20,
  `new_value` need only be writable to `first`) — a subtle
  distinction that mostly matters for proxy iterators.

### Example

```cpp
#include <algorithm>
#include <array>
#include <functional>
#include <iostream>

int main()
{
    std::array<int, 10> s{5, 7, 4, 2, 8, 6, 1, 9, 0, 3};

    std::replace(s.begin(), s.end(), 8, 88);

    for (int a : s)
        std::cout << a << ' ';
    std::cout << '\n';

    std::replace_if(s.begin(), s.end(),
                    [](int x) { return x < 5; }, 55);

    for (int a : s)
        std::cout << a << ' ';
    std::cout << '\n';
}
```

```text
5 7 4 2 88 6 1 9 0 3 
5 7 55 55 88 6 55 9 55 55
```

### Reference

Full declarations:

```cpp skip
template< class ForwardIt, class T >
void replace( ForwardIt first, ForwardIt last,
              const T& old_value, const T& new_value );  // (until C++20)
template< class ForwardIt, class T >
constexpr void replace( ForwardIt first, ForwardIt last,
                        const T& old_value, const T& new_value );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class T >
void replace( ExecutionPolicy&& policy, ForwardIt first, ForwardIt last,
              const T& old_value, const T& new_value );  // (2) (since C++17)
template< class ForwardIt, class UnaryPredicate, class T >
void replace_if( ForwardIt first, ForwardIt last,
                 UnaryPredicate p, const T& new_value );  // (until C++20)
template< class ForwardIt, class UnaryPredicate, class T >
constexpr void replace_if( ForwardIt first, ForwardIt last,
                           UnaryPredicate p, const T& new_value );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt,
          class UnaryPredicate, class T >
void replace_if( ExecutionPolicy&& policy, ForwardIt first, ForwardIt last,
                 UnaryPredicate p, const T& new_value );  // (4) (since C++17)
```

`ForwardIt` must meet LegacyForwardIterator. LWG 283 (applied
retroactively to C++98): `T` was originally required to be
CopyAssignable (and EqualityComparable for `replace`), which isn't
always the right requirement since the value type of `ForwardIt` need
not be `T`; the corrected requirement is that `*first = new_value` be
valid.

### See also

- **replace_copy** — copies a range, replacing matching elements
- **remove** — erase-remove idiom for dropping elements, not replacing
- **ranges::replace** — constrained version (C++20)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/replace*
