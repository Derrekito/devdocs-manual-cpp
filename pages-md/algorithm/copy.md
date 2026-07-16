# std::copy, std::copy_if

Copies `[first, last)` to a destination range starting at `d_first`.
The destination must not overlap the source in a way that reads an
element after it's already been overwritten: if `d_first` falls
inside `[first, last)`, or the two ranges otherwise overlap, behavior
is undefined. `std::copy` is safe when copying **left** (destination
start is outside the source range); use `std::copy_backward` when
copying **right** (destination end is outside the source range).
`std::copy_if` copies only elements a predicate accepts, preserving
their relative order, and has the same overlap restriction.

```cpp skip
std::copy(first, last, d_first);                    // copy all
std::copy(policy, first, last, d_first);             // parallel        (since C++17)
std::copy_if(first, last, d_first, pred);            // copy matching   (since C++11)
std::copy_if(policy, first, last, d_first, pred);    // parallel + pred (since C++17)
```

Both non-policy forms are `constexpr` since C++20.

### What you provide

- **first, last** — the source range (LegacyInputIterator).
- **d_first** — start of the destination (LegacyOutputIterator); needs
  room for `last - first` elements, or use an inserter (e.g.
  `std::back_inserter`) to grow the container as it copies.
- **pred** — `copy_if` only: a callable returning `true` for elements
  to keep. Must not modify what it's given.
- **policy** — an execution policy (C++17) for parallel execution;
  requires ForwardIt on both sides, not just InputIt/OutputIt.

### Guarantees and costs

- `std::copy`: exactly `N` assignments, where `N =
  std::distance(first, last)`.
- `std::copy_if`: exactly `N` predicate applications, at most `N`
  assignments.
- Real implementations fast-path `std::copy` to `std::memmove` when
  the value type is TriviallyCopyable and the iterators are
  contiguous — it's not a naive element-by-element loop in practice.
- Parallel overloads: if a predicate/assignment throws,
  `std::terminate` is called; allocation failure throws
  `std::bad_alloc`.
- Returns an output iterator one past the last element written.

### Gotchas

- Overlapping source and destination is UB for both algorithms, not
  just slow — reach for `std::copy_backward` when shifting elements
  rightward within the same container.
- `d_first` needs pre-existing room; copying into an empty
  `std::vector` without `std::back_inserter` or a prior `resize` is a
  common out-of-bounds bug.
- `copy_if` is stable (keeps relative order of copied elements), but
  that guarantee doesn't extend to what's left in the source — nothing
  is removed from `[first, last)`.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <numeric>
#include <vector>

int main()
{
    std::vector<int> from_vector(10);
    std::iota(from_vector.begin(), from_vector.end(), 0);

    std::vector<int> to_vector;
    std::copy(from_vector.begin(), from_vector.end(),
              std::back_inserter(to_vector));

    std::cout << "to_vector contains: ";
    std::copy(to_vector.begin(), to_vector.end(),
              std::ostream_iterator<int>(std::cout, " "));
    std::cout << '\n';

    std::cout << "odd numbers in to_vector are: ";
    std::copy_if(to_vector.begin(), to_vector.end(),
                 std::ostream_iterator<int>(std::cout, " "),
                 [](int x) { return x % 2 != 0; });
    std::cout << '\n';
}
```

```text
to_vector contains: 0 1 2 3 4 5 6 7 8 9 
odd numbers in to_vector are: 1 3 5 7 9
```

### Reference

Full declarations:

```cpp skip
template< class InputIt, class OutputIt >
OutputIt copy( InputIt first, InputIt last, OutputIt d_first );  // (1) (constexpr since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2 >
ForwardIt2 copy( ExecutionPolicy&& policy,
                 ForwardIt1 first, ForwardIt1 last, ForwardIt2 d_first );  // (2) (since C++17)
template< class InputIt, class OutputIt, class UnaryPredicate >
OutputIt copy_if( InputIt first, InputIt last, OutputIt d_first,
                  UnaryPredicate pred );  // (3) (since C++11) (constexpr since C++20)
template< class ExecutionPolicy,
          class ForwardIt1, class ForwardIt2, class UnaryPredicate >
ForwardIt2 copy_if( ExecutionPolicy&& policy,
                    ForwardIt1 first, ForwardIt1 last, ForwardIt2 d_first,
                    UnaryPredicate pred );  // (4) (since C++17)
```

`InputIt` must meet LegacyInputIterator, `OutputIt`
LegacyOutputIterator, and the policy overloads' `ForwardIt1`/
`ForwardIt2` LegacyForwardIterator. For the `ExecutionPolicy`
overloads there may be a performance cost if the value type isn't
MoveConstructible. LWG 2039/2044 (applied retroactively to C++11):
`copy_if`'s return value and stability were originally unspecified;
both are now guaranteed.

### See also

- **copy_backward** — copies a range in backwards order, safe for
  right-overlapping destinations
- **reverse_copy** — copies a range in reverse
- **copy_n** — copies a fixed count of elements (C++11)
- **fill** — assigns the same value to every element in a range
- **remove_copy** — copies a range, omitting elements equal to a value
- **ranges::copy** — constrained version (C++20)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/copy*
