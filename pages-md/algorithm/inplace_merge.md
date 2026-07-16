# std::inplace_merge

```cpp
template< class BidirIt >
void inplace_merge( BidirIt first, BidirIt middle, BidirIt last );  // (1) (constexpr since C++26)
template< class ExecutionPolicy, class BidirIt >
void inplace_merge( ExecutionPolicy&& policy,
                    BidirIt first, BidirIt middle, BidirIt last );  // (2) (since C++17)
template< class BidirIt, class Compare >
void inplace_merge( BidirIt first, BidirIt middle, BidirIt last, Compare comp );  // (3) (constexpr since C++26)
template< class ExecutionPolicy, class BidirIt, class Compare >
void inplace_merge( ExecutionPolicy&& policy,
                    BidirIt first, BidirIt middle, BidirIt last, Compare comp );  // (4) (since C++17)
```

Merges two consecutive sorted ranges `[``first``,``middle``)` and
`[``middle``,``last``)` into one sorted range `[``first``,``last``)`.

A sequence `[``first``,``last``)` is said to be *sorted* with respect to a
comparator `comp` if for any iterator `it` pointing to the sequence and any
non-negative integer `n` such that `it + n` is a valid iterator pointing to an
element of the sequence, `comp(*(it + n), *it)` evaluates to `false`.

This merge is *stable*, which means that for equivalent elements in the original
two ranges, the elements from the first range (preserving their original order)
precede the elements from the second range (preserving their original order).

1) Elements are compared using `operator<` and the ranges must be sorted with
   respect to the same.

3) Elements are compared using the given binary comparison function `comp` and
   the ranges must be sorted with respect to the same.

2,4) Same as (1,3), but executed according to `policy`. These overloads do not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

### Parameters

- **first** — the beginning of the first sorted range
- **middle** — the end of the first sorted range and the beginning of the second
- **last** — the end of the second sorted range
- **policy** — the execution policy to use. See execution policy for details.
- **comp** — comparison function object (i.e. an object that satisfies the
  requirements of Compare) which returns ​`true` if the first argument is *less*
  than (i.e. is ordered *before*) the second. The signature of the comparison
  function should be equivalent to the following: `bool cmp(const Type1& a,
  const Type2& b);` While the signature does not need to have const&, the
  function must not modify the objects passed to it and must be able to accept
  all values of type (possibly const) `Type1` and `Type2` regardless of value
  category (thus, `Type1&` is not allowed, nor is `Type1` unless for `Type1` a
  move is equivalent to a copy(since C++11)). The types Type1 and Type2 must be
  such that an object of type BidirIt can be dereferenced and then implicitly
  converted to both of them. ​

**Type requirements**

**-`BidirIt` must meet the requirements of ValueSwappable and LegacyBidirectionalIterator.**

**-The type of dereferenced `BidirIt` must meet the requirements of MoveAssignable and MoveConstructible.**

### Return value

(none)

### Complexity

Given `N = std::distance(first, last)`,

1,3) Exactly `N - 1` comparisons if enough additional memory is available. If
   the memory is insufficient, \(\scriptsize O(N log(N))\)O(N log(N))
   comparisons.

2,4) \(\scriptsize O(N log(N))\)O(N log(N)) comparisons.

### Exceptions

The overloads with a template parameter named `ExecutionPolicy` report errors as
follows:

- If execution of a function invoked as part of the algorithm throws an
  exception and `ExecutionPolicy` is one of the standard policies,
  `std::terminate` is called. For any other `ExecutionPolicy`, the behavior is
  implementation-defined.
- If the algorithm fails to allocate memory, `std::bad_alloc` is thrown.

### Notes

This function attempts to allocate a temporary buffer. If the allocation fails,
the less efficient algorithm is chosen.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_constexpr_algorithms` | 202306L | `constexpr` stable sorting

### Possible implementation

See the implementations in libstdc++ and libc++.

### Example

The following code is an implementation of merge sort.

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

template<class Iter>
void merge_sort(Iter first, Iter last)
{
    if (last - first > 1)
    {
        Iter middle = first + (last - first) / 2;
        merge_sort(first, middle);
        merge_sort(middle, last);
        std::inplace_merge(first, middle, last);
    }
}

int main()
{
    std::vector<int> v{8, 2, -2, 0, 11, 11, 1, 7, 3};
    merge_sort(v.begin(), v.end());
    for (const auto& n : v)
        std::cout << n << ' ';
    std::cout << '\n';
}
```

Output:

```text
-2 0 1 2 3 7 8 11 11
```

### See also

- **merge** — merges two sorted ranges (function template)
- **sort** — sorts a range into ascending order (function template)
- **stable_sort** — sorts a range of elements while preserving order between
  equal elements (function template)
- **ranges::inplace_merge (C++20)** — merges two ordered ranges in-place
  (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/inplace_merge*
