# std::set_union

```cpp
template< class InputIt1, class InputIt2, class OutputIt >
OutputIt set_union( InputIt1 first1, InputIt1 last1,
                    InputIt2 first2, InputIt2 last2, OutputIt d_first );  // (until C++20)
template< class InputIt1, class InputIt2, class OutputIt >
constexpr OutputIt set_union( InputIt1 first1, InputIt1 last1,
                              InputIt2 first2, InputIt2 last2,
                              OutputIt d_first );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1,
          class ForwardIt2, class ForwardIt3 >
ForwardIt3 set_union( ExecutionPolicy&& policy,
                      ForwardIt1 first1, ForwardIt1 last1,
                      ForwardIt2 first2, ForwardIt2 last2, ForwardIt3 d_first );  // (2) (since C++17)
template< class InputIt1, class InputIt2, class OutputIt, class Compare >
OutputIt set_union( InputIt1 first1, InputIt1 last1,
                    InputIt2 first2, InputIt2 last2,
                    OutputIt d_first, Compare comp );  // (until C++20)
template< class InputIt1, class InputIt2, class OutputIt, class Compare >
constexpr OutputIt set_union( InputIt1 first1, InputIt1 last1,
                              InputIt2 first2, InputIt2 last2,
                              OutputIt d_first, Compare comp );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1,
          class ForwardIt2, class ForwardIt3, class Compare >
ForwardIt3 set_union( ExecutionPolicy&& policy,
                      ForwardIt1 first1, ForwardIt1 last1,
                      ForwardIt2 first2, ForwardIt2 last2,
                      ForwardIt3 d_first, Compare comp );  // (4) (since C++17)
```

Constructs a sorted union beginning at `d_first` consisting of the set of
elements present in one or both sorted ranges `[``first1``,``last1``)` and
`[``first2``,``last2``)`.

If `[``first1``,``last1``)` contains `m` elements that are equivalent to each
other and `[``first2``,``last2``)` contains `n` elements that are equivalent to
them, then all `m` elements will be copied from `[``first1``,``last1``)` to the
output range, preserving order, and then the final `std::max(n - m, 0)` elements
will be copied from `[``first2``,``last2``)` to the output range, also
preserving order.

1) Elements are compared using `operator<` and the ranges must be sorted with
   respect to the same.

3) Elements are compared using the given binary comparison function `comp` and
   the ranges must be sorted with respect to the same.

2,4) Same as (1,3), but executed according to `policy`. These overloads do not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

If either of the input ranges is not sorted (using `operator<` or `comp`,
respectively) or overlaps with the output range, the behavior is undefined.

### Parameters

- **first1, last1** — the first input sorted range
- **first2, last2** — the second input sorted range
- **d_first** — the beginning of the output range
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
  such that objects of types InputIt1 and InputIt2 can be dereferenced and then
  implicitly converted to both Type1 and Type2. ​

**Type requirements**

**-`InputIt1, InputIt2` must meet the requirements of LegacyInputIterator.**

**-`ForwardIt1, ForwardIt2, ForwardIt3` must meet the requirements of LegacyForwardIterator.**

**-`OutputIt` must meet the requirements of LegacyOutputIterator.**

### Return value

Iterator past the end of the constructed range.

### Complexity

Given `M` and `N` as `std::distance(first1, last1)` and `std::distance(first2,
last2)` respectively:

1,2) at most `2·(M + N) - 1` comparisons using `operator<`.

3,4) at most `2·(M + N) - 1` applications of the predicate `p`.

### Exceptions

The overloads with a template parameter named `ExecutionPolicy` report errors as
follows:

- If execution of a function invoked as part of the algorithm throws an
  exception and `ExecutionPolicy` is one of the standard policies,
  `std::terminate` is called. For any other `ExecutionPolicy`, the behavior is
  implementation-defined.
- If the algorithm fails to allocate memory, `std::bad_alloc` is thrown.

### Notes

This algorithm performs a similar task as `std::merge` does. Both consume two
sorted input ranges and produce a sorted output with elements from both inputs.
The difference between these two algorithms is with handling values from both
input ranges which compare equivalent (see notes on LessThanComparable). If any
equivalent values appeared `n` times in the first range and `m` times in the
second, `std::merge` would output all `n + m` occurrences whereas
`std::set_union` would output `std::max(n, m)` ones only. So `std::merge`
outputs exactly `std::distance(first1, last1) + std::distance(first2, last2)`
values and `std::set_union` may produce fewer.

### Possible implementation

```cpp
template<class InputIt1, class InputIt2, class OutputIt>
OutputIt set_union(InputIt1 first1, InputIt1 last1,
                   InputIt2 first2, InputIt2 last2, OutputIt d_first)
{
    for (; first1 != last1; ++d_first)
    {
        if (first2 == last2)
            return std::copy(first1, last1, d_first);

        if (*first2 < *first1)
            *d_first = *first2++;
        else
        {
            *d_first = *first1;
            if (!(*first1 < *first2))
                ++first2;
            ++first1;
        }
    }
    return std::copy(first2, last2, d_first);
}
```

```cpp
template<class InputIt1, class InputIt2, class OutputIt, class Compare>
OutputIt set_union(InputIt1 first1, InputIt1 last1,
                   InputIt2 first2, InputIt2 last2, OutputIt d_first, Compare comp)
{
    for (; first1 != last1; ++d_first)
    {
        if (first2 == last2)
            // Finished range 2, include the rest of range 1:
            return std::copy(first1, last1, d_first);

        if (comp(*first2, *first1))
            *d_first = *first2++;
        else
        {
            *d_first = *first1;
            if (!comp(*first1, *first2)) // Equivalent => don't need to include *first2.
                ++first2;
            ++first1;
        }
    }
    // Finished range 1, include the rest of range 2:
    return std::copy(first2, last2, d_first);
}
```

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <vector>

void println(std::vector<int> const& v)
{
    for (int i : v)
        std::cout << i << ' ';
    std::cout << '\n';
}

int main()
{
    std::vector<int> v1, v2, dest;

    v1 = {1, 2, 3, 4, 5};
    v2 = {3, 4, 5, 6, 7};

    std::set_union(v1.cbegin(), v1.cend(),
                   v2.cbegin(), v2.cend(),
                   std::back_inserter(dest));
    println(dest);

    dest.clear();

    v1 = {1, 2, 3, 4, 5, 5, 5};
    v2 = {3, 4, 5, 6, 7};

    std::set_union(v1.cbegin(), v1.cend(),
                   v2.cbegin(), v2.cend(),
                   std::back_inserter(dest));
    println(dest);
}
```

Output:

```text
1 2 3 4 5 6 7
1 2 3 4 5 5 5 6 7
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 291 | C++98 | it was unspecified how to handle equivalent elements in the
      input ranges | specified

### See also

- **includes** — returns `true` if one sequence is a subsequence of another
  (function template)
- **merge** — merges two sorted ranges (function template)
- **set_difference** — computes the difference between two sets (function
  template)
- **set_intersection** — computes the intersection of two sets (function
  template)
- **set_symmetric_difference** — computes the symmetric difference between two
  sets (function template)
- **ranges::set_union (C++20)** — computes the union of two sets (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/set_union*
