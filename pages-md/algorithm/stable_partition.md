# std::stable_partition

```cpp
template< class BidirIt, class UnaryPredicate >
BidirIt stable_partition( BidirIt first, BidirIt last, UnaryPredicate p );  // (1) (constexpr since C++26)
template< class ExecutionPolicy, class BidirIt, class UnaryPredicate >
BidirIt stable_partition( ExecutionPolicy&& policy,
                          BidirIt first, BidirIt last, UnaryPredicate p );  // (2) (since C++17)
```

1) Reorders the elements in the range `[``first``,``last``)` in such a way that
   all elements for which the predicate `p` returns `true` precede the elements
   for which predicate `p` returns `false`. Relative order of the elements is
   preserved.

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
  of `BidirIt`, regardless of value category, and must not modify `v`. Thus, a
  parameter type of `VT&`is not allowed, nor is `VT` unless for `VT` a move is
  equivalent to a copy(since C++11). ​

**Type requirements**

**-`BidirIt` must meet the requirements of ValueSwappable and LegacyBidirectionalIterator.**

**-The type of dereferenced `BidirIt` must meet the requirements of MoveAssignable and MoveConstructible.**

**-`UnaryPredicate` must meet the requirements of Predicate.**

### Return value

Iterator to the first element of the second group

### Complexity

Given \(\scriptsize N\)N as `std::distance(first, last)`:

1) Exactly \(\scriptsize N\)N applications of the predicate and \(\scriptsize
   O(N)\)O(N) swaps if there is enough extra memory. If memory is insufficient,
   at most \(\scriptsize N log(N)\)N log(N) swaps.

2) \(\scriptsize N log(N)\)N log(N) swaps and \(\scriptsize O(N)\)O(N)
   applications of the predicate.

### Exceptions

The overload with a template parameter named `ExecutionPolicy` reports errors as
follows:

- If execution of a function invoked as part of the algorithm throws an
  exception and `ExecutionPolicy` is one of the standard policies,
  `std::terminate` is called. For any other `ExecutionPolicy`, the behavior is
  implementation-defined.
- If the algorithm fails to allocate memory, `std::bad_alloc` is thrown.

### Notes

This function attempts to allocate a temporary buffer. If the allocation fails,
the less efficient algorithm is chosen.

Implementations in libc++ and libstdc++ also accept ranges denoted by
LegacyForwardIterators as an extension.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_constexpr_algorithms` | 202306L | `constexpr` stable sorting

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> v {0, 0, 3, -1, 2, 4, 5, 0, 7};
    std::stable_partition(v.begin(), v.end(), [](int n) { return n > 0; });
    for (int n : v)
        std::cout << n << ' ';
    std::cout << '\n';
}
```

Output:

```text
3 2 4 5 7 0 0 -1 0
```

### See also

- **partition** — divides a range of elements into two groups (function
  template)
- **ranges::stable_partition (C++20)** — divides elements into two groups while
  preserving their relative order (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/stable_partition*
