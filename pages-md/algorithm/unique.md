# std::unique

Collapses each run of consecutive equal elements down to its first
member, shifting survivors to the front of the range. It does **not**
shrink the container — pair it with `erase` to actually drop the tail.

```cpp skip
std::unique(first, last);              // uses operator==
std::unique(first, last, pred);        // your equivalence test
std::unique(policy, first, last);      // parallel        (since C++17)
std::unique(policy, first, last, pred);// parallel + pred (since C++17)
```

Since C++20 the non-policy forms are `constexpr`.

### What you provide

- **first, last** — forward iterators (a `vector`, `deque`, `list`,
  `array`, `std::string`, or C array all qualify).
- **pred** — a binary predicate answering *are these two elements
  equal?*, returning `bool`. It must be an equivalence relation
  (reflexive, symmetric, transitive) and must not modify its
  arguments; otherwise behavior is undefined. Default is `operator==`.
- **policy** — an execution policy such as `std::execution::par`
  (C++17) to run in parallel.

### Guarantees and costs

- For a nonempty range, exactly `distance(first, last) - 1`
  applications of the predicate.
- Relative order of the surviving elements is preserved; the
  container's *physical* size is unchanged — only the *logical* end
  moves.
- Returns an iterator to the new logical end. Elements from there to
  the old `last` are still dereferenceable but hold unspecified
  values.
- Parallel overloads: a throwing predicate calls `std::terminate`;
  allocation failure throws `std::bad_alloc`.

### Gotchas

- It only collapses **adjacent** duplicates. On unsorted input,
  non-adjacent repeats survive untouched — sort first if you want all
  duplicates gone (the sort-unique-erase idiom).
- Forgetting the trailing `erase` is the classic bug: the container
  keeps its original size, with unspecified junk in the tail.
- Don't confuse this with C++20's free function `std::erase(container,
  value)`, which removes every element equal to a given *value*, not
  adjacent duplicates.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> v{1, 2, 1, 1, 3, 3, 3, 4, 5, 4};

    auto new_end = std::unique(v.begin(), v.end());
    v.erase(new_end, v.end());

    for (int x : v)
        std::cout << x << ' ';
    std::cout << '\n';
}
```

```text
1 2 1 3 4 5 4
```

### Reference

Full declarations:

```cpp skip
template< class ForwardIt >
ForwardIt unique( ForwardIt first, ForwardIt last );  // (until C++20)
template< class ForwardIt >
constexpr ForwardIt unique( ForwardIt first, ForwardIt last );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt >
ForwardIt unique( ExecutionPolicy&& policy,
                  ForwardIt first, ForwardIt last );  // (2) (since C++17)
template< class ForwardIt, class BinaryPredicate >
ForwardIt unique( ForwardIt first, ForwardIt last,
                  BinaryPredicate p );  // (until C++20)
template< class ForwardIt, class BinaryPredicate >
constexpr ForwardIt unique( ForwardIt first, ForwardIt last,
                            BinaryPredicate p );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class BinaryPredicate >
ForwardIt unique( ExecutionPolicy&& policy,
                  ForwardIt first, ForwardIt last,
                  BinaryPredicate p );  // (4) (since C++17)
```

`ForwardIt` must meet LegacyForwardIterator, and its dereferenced type
must be MoveAssignable. The predicate's parameter types must accept
all value categories of the dereferenced iterator (no plain non-const
reference parameter); a defect report (LWG 202, applied to C++98)
clarified that a non-equivalence predicate is undefined behavior
rather than merely unclear.

### See also

- **adjacent_find** — finds the first adjacent equal pair without
  removing anything
- **unique_copy** — writes deduplicated elements to a separate range
- **remove, remove_if** — removes elements matching a value/predicate
- **ranges::unique (C++20)** — constrained version with projections

---
*Source: https://en.cppreference.com/w/cpp/algorithm/unique*
