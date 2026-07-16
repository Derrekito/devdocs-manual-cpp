# std::partition

```cpp
template< class ForwardIt, class UnaryPredicate >
ForwardIt partition( ForwardIt first, ForwardIt last, UnaryPredicate p );  // (until C++20)
template< class ForwardIt, class UnaryPredicate >
constexpr ForwardIt partition( ForwardIt first, ForwardIt last,
                               UnaryPredicate p );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class UnaryPredicate >
ForwardIt partition( ExecutionPolicy&& policy,
                     ForwardIt first, ForwardIt last, UnaryPredicate p );  // (2) (since C++17)
```

1) Reorders the elements in the range `[``first``,``last``)` in such a way that
   all elements for which the predicate `p` returns `true` precede the elements
   for which predicate `p` returns `false`. Relative order of the elements is
   not preserved.

2) Same as (1), but executed according to `policy`. This overload does not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

### Parameters

- **first, last** — the range of elements to reorder
- **policy** — the execution policy to use. See execution policy for details.
- **p** — unary predicate which returns ​`true` if the element should be ordered
  before other elements. The expression `p(v)` must be convertible to `bool` for
  every argument `v` of type (possibly const) `VT`, where `VT` is the value type
  of `ForwardIt`, regardless of value category, and must not modify `v`. Thus, a
  parameter type of `VT&`is not allowed, nor is `VT` unless for `VT` a move is
  equivalent to a copy(since C++11). ​

**Type requirements**

**-`ForwardIt` must meet the requirements of ValueSwappable and LegacyForwardIterator. However, the operation is more efficient if `ForwardIt` also satisfies the requirements of LegacyBidirectionalIterator.**

**-`UnaryPredicate` must meet the requirements of Predicate.**

### Return value

Iterator to the first element of the second group.

### Complexity

Given `N` = `std::distance(first, last)`,

1) Exactly `N` applications of `p`. At most `N / 2` swaps if `ForwardIt` meets
   the requirements of LegacyBidirectionalIterator, and at most N swaps
   otherwise.

2) `O(N·log N)` swaps and `O(N)` applications of `p`.

### Exceptions

The overload with a template parameter named `ExecutionPolicy` reports errors as
follows:

- If execution of a function invoked as part of the algorithm throws an
  exception and `ExecutionPolicy` is one of the standard policies,
  `std::terminate` is called. For any other `ExecutionPolicy`, the behavior is
  implementation-defined.
- If the algorithm fails to allocate memory, `std::bad_alloc` is thrown.

### Possible implementation

Implements overload (1) preserving C++11 compatibility.

```cpp
template<class ForwardIt, class UnaryPredicate>
ForwardIt partition(ForwardIt first, ForwardIt last, UnaryPredicate p)
{
    first = std::find_if_not(first, last, p);
    if (first == last)
        return first;

    for (auto i = std::next(first); i != last; ++i)
        if (p(*i))
        {
            std::iter_swap(i, first);
            ++first;
        }

    return first;
}
```

### Example

```cpp
#include <algorithm>
#include <forward_list>
#include <iostream>
#include <iterator>
#include <vector>

template<class ForwardIt>
void quicksort(ForwardIt first, ForwardIt last)
{
    if (first == last)
        return;

    auto pivot = *std::next(first, std::distance(first, last) / 2);
    auto middle1 = std::partition(first, last, [pivot](const auto& em)
    {
        return em < pivot;
    });
    auto middle2 = std::partition(middle1, last, [pivot](const auto& em)
    {
        return !(pivot < em);
    });

    quicksort(first, middle1);
    quicksort(middle2, last);
}

int main()
{
    std::vector<int> v {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
    std::cout << "Original vector: ";
    for (int elem : v)
        std::cout << elem << ' ';

    auto it = std::partition(v.begin(), v.end(), [](int i) {return i % 2 == 0;});

    std::cout << "\nPartitioned vector: ";
    std::copy(std::begin(v), it, std::ostream_iterator<int>(std::cout, " "));
    std::cout << "* ";
    std::copy(it, std::end(v), std::ostream_iterator<int>(std::cout, " "));

    std::forward_list<int> fl {1, 30, -4, 3, 5, -4, 1, 6, -8, 2, -5, 64, 1, 92};
    std::cout << "\nUnsorted list: ";
    for (int n : fl)
        std::cout << n << ' ';

    quicksort(std::begin(fl), std::end(fl));
    std::cout << "\nSorted using quicksort: ";
    for (int fi : fl)
        std::cout << fi << ' ';
    std::cout << '\n';
}
```

Possible output:

```text
Original vector: 0 1 2 3 4 5 6 7 8 9
Partitioned vector: 0 8 2 6 4 * 5 3 7 1 9
Unsorted list: 1 30 -4 3 5 -4 1 6 -8 2 -5 64 1 92
Sorted using quicksort: -8 -5 -4 -4 1 1 1 2 3 5 6 30 64 92
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 498 | C++98 | `std::partition` required `first` and `last` to be
      LegacyBidirectionalIterator | only required to be LegacyForwardIterator
      (the complexity requirement for non-bidirectional iterators is weaker)

### See also

- **is_partitioned (C++11)** — determines if the range is partitioned by the
  given predicate (function template)
- **stable_partition** — divides elements into two groups while preserving their
  relative order (function template)
- **ranges::partition (C++20)** — divides a range of elements into two groups
  (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/partition*
