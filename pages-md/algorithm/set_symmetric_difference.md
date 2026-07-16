# std::set_symmetric_difference

```cpp
template< class InputIt1, class InputIt2, class OutputIt >
OutputIt set_symmetric_difference( InputIt1 first1, InputIt1 last1,
                                   InputIt2 first2, InputIt2 last2,
                                   OutputIt d_first );  // (until C++20)
template< class InputIt1, class InputIt2, class OutputIt >
constexpr OutputIt set_symmetric_difference( InputIt1 first1, InputIt1 last1,
                                             InputIt2 first2, InputIt2 last2,
                                             OutputIt d_first );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1,
          class ForwardIt2, class ForwardIt3 >
ForwardIt3 set_symmetric_difference( ExecutionPolicy&& policy,
                                     ForwardIt1 first1, ForwardIt1 last1,
                                     ForwardIt2 first2, ForwardIt2 last2,
                                     ForwardIt3 d_first );  // (2) (since C++17)
template< class InputIt1, class InputIt2, class OutputIt, class Compare >
OutputIt set_symmetric_difference( InputIt1 first1, InputIt1 last1,
                                   InputIt2 first2, InputIt2 last2,
                                   OutputIt d_first, Compare comp );  // (until C++20)
template< class InputIt1, class InputIt2, class OutputIt, class Compare >
constexpr OutputIt set_symmetric_difference( InputIt1 first1, InputIt1 last1,
                                             InputIt2 first2, InputIt2 last2,
                                             OutputIt d_first, Compare comp );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1,
          class ForwardIt2, class ForwardIt3, class Compare >
ForwardIt3 set_symmetric_difference( ExecutionPolicy&& policy,
                                     ForwardIt1 first1, ForwardIt1 last1,
                                     ForwardIt2 first2, ForwardIt2 last2,
                                     ForwardIt3 d_first, Compare comp );  // (4) (since C++17)
```

Computes symmetric difference of two sorted ranges: the elements that are found
in either of the ranges, but not in both of them are copied to the range
beginning at `d_first`. The output range is also sorted.

If `[``first1``,``last1``)` contains `m` elements that are equivalent to each
other and `[``first2``,``last2``)` contains `n` elements that are equivalent to
them, then `std::abs(m - n)` of those elements will be copied to the output
range, preserving order:

- if `m > n`, the final `m - n` of these elements from `[``first1``,``last1``)`.
- if `m < n`, the final `n - m` of these elements from `[``first2``,``last2``)`.

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

- **first1, last1** — the first sorted range of elements
- **first2, last2** — the second sorted range of elements
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

**-`OutputIt` must meet the requirements of LegacyOutputIterator.**

**-`ForwardIt1, ForwardIt2, ForwardIt3` must meet the requirements of LegacyForwardIterator.**

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

### Possible implementation

```cpp
template<class InputIt1, class InputIt2, class OutputIt>
OutputIt set_symmetric_difference(InputIt1 first1, InputIt1 last1,
                                  InputIt2 first2, InputIt2 last2, OutputIt d_first)
{
    while (first1 != last1)
    {
        if (first2 == last2)
            return std::copy(first1, last1, d_first);

        if (*first1 < *first2)
            *d_first++ = *first1++;
        else
        {
            if (*first2 < *first1)
                *d_first++ = *first2;
            else
                ++first1;
            ++first2;
        }
    }
    return std::copy(first2, last2, d_first);
}
```

```cpp
template<class InputIt1, class InputIt2, class OutputIt, class Compare>
OutputIt set_symmetric_difference(InputIt1 first1, InputIt1 last1,
                                  InputIt2 first2, InputIt2 last2,
                                  OutputIt d_first, Compare comp)
{
    while (first1 != last1)
    {
        if (first2 == last2)
            return std::copy(first1, last1, d_first);

        if (comp(*first1, *first2))
            *d_first++ = *first1++;
        else
        {
            if (comp(*first2, *first1))
                *d_first++ = *first2;
            else
                ++first1;
            ++first2;
        }
    }
    return std::copy(first2, last2, d_first);
}
```

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <vector>

int main()
{
    std::vector<int> v1{1, 2, 3, 4, 5, 6, 7, 8};
    std::vector<int> v2{5, 7, 9, 10};
    std::sort(v1.begin(), v1.end());
    std::sort(v2.begin(), v2.end());

    std::vector<int> v_symDifference;

    std::set_symmetric_difference(v1.begin(), v1.end(), v2.begin(), v2.end(),
                                  std::back_inserter(v_symDifference));

    for (int n : v_symDifference)
        std::cout << n << ' ';
    std::cout << '\n';
}
```

Output:

```text
1 2 3 4 6 8 9 10
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
- **set_difference** — computes the difference between two sets (function
  template)
- **set_union** — computes the union of two sets (function template)
- **set_intersection** — computes the intersection of two sets (function
  template)
- **ranges::set_symmetric_difference (C++20)** — computes the symmetric
  difference between two sets (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/set_symmetric_difference*
