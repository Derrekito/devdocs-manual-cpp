# std::merge

```cpp
template< class InputIt1, class InputIt2, class OutputIt >
OutputIt merge( InputIt1 first1, InputIt1 last1,
                InputIt2 first2, InputIt2 last2,
                OutputIt d_first );  // (until C++20)
template< class InputIt1, class InputIt2, class OutputIt >
constexpr OutputIt merge( InputIt1 first1, InputIt1 last1,
                          InputIt2 first2, InputIt2 last2,
                          OutputIt d_first );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2,
          class ForwardIt3 >
ForwardIt3 merge( ExecutionPolicy&& policy,
                  ForwardIt1 first1, ForwardIt1 last1,
                  ForwardIt2 first2, ForwardIt2 last2,
                  ForwardIt3 d_first );  // (2) (since C++17)
template< class InputIt1, class InputIt2, class OutputIt, class Compare >
OutputIt merge( InputIt1 first1, InputIt1 last1,
                InputIt2 first2, InputIt2 last2,
                OutputIt d_first, Compare comp );  // (until C++20)
template< class InputIt1, class InputIt2, class OutputIt, class Compare >
constexpr OutputIt merge( InputIt1 first1, InputIt1 last1,
                          InputIt2 first2, InputIt2 last2,
                          OutputIt d_first, Compare comp );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2,
          class ForwardIt3, class Compare >
ForwardIt3 merge( ExecutionPolicy&& policy,
                  ForwardIt1 first1, ForwardIt1 last1,
                  ForwardIt2 first2, ForwardIt2 last2,
                  ForwardIt3 d_first, Compare comp );  // (4) (since C++17)
```

Merges two *sorted* ranges `[``first1``,``last1``)` and `[``first2``,``last2``)`
into one *sorted* range beginning at `d_first`.

A sequence is said to be *sorted* with respect to a comparator `comp` if for any
iterator `it` pointing to the sequence and any non-negative integer `n` such
that `it + n` is a valid iterator pointing to an element of the sequence,
`comp(*(it + n), *it)` evaluates to `false`.

1) Elements are compared using `operator<`.

3) Elements are compared using the given binary comparison function `comp`.

2,4) Same as (1,3), but executed according to `policy`. These overloads do not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

This merge function is *stable*, which means that for equivalent elements in the
original two ranges, the elements from the first range (preserving their
original order) precede the elements from the second range (preserving their
original order).

The behavior is undefined if the destination range overlaps either of the input
ranges (the input ranges may overlap each other).

### Parameters

- **first1, last1** — the first range of elements to merge
- **first2, last2** — the second range of elements to merge
- **d_first** — the beginning of the destination range
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

An output iterator to element past the last element copied.

### Complexity

Given `N` as `std::distance(first1, last1) + std::distance(first2, last2)`:

1) at most `N - 1` comparisons using `operator<`

2) `O(N)` comparisons using `operator<`

3) at most `N - 1` comparisons using `comp`

4) `O(N)` comparisons using `comp`

### Exceptions

The overloads with a template parameter named `ExecutionPolicy` report errors as
follows:

- If execution of a function invoked as part of the algorithm throws an
  exception and `ExecutionPolicy` is one of the standard policies,
  `std::terminate` is called. For any other `ExecutionPolicy`, the behavior is
  implementation-defined.
- If the algorithm fails to allocate memory, `std::bad_alloc` is thrown.

### Notes

This algorithm performs a similar task as `std::set_union` does. Both consume
two sorted input ranges and produce a sorted output with elements from both
inputs. The difference between these two algorithms is with handling values from
both input ranges which compare equivalent (see notes on LessThanComparable). If
any equivalent values appeared `n` times in the first range and `m` times in the
second, `std::merge` would output all `n + m` occurrences whereas
`std::set_union` would output `std::max(n, m)` ones only. So `std::merge`
outputs exactly `std::distance(first1, last1) + std::distance(first2, last2)`
values and `std::set_union` may produce fewer.

### Possible implementation

See also the implementations in libstdc++ and libc++.

```cpp
template<class InputIt1, class InputIt2, class OutputIt>
OutputIt merge(InputIt1 first1, InputIt1 last1,
               InputIt2 first2, InputIt2 last2,
               OutputIt d_first)
{
    for (; first1 != last1; ++d_first)
    {
        if (first2 == last2)
            return std::copy(first1, last1, d_first);

        if (*first2 < *first1)
        {
            *d_first = *first2;
            ++first2;
        }
        else
        {
            *d_first = *first1;
            ++first1;
        }
    }
    return std::copy(first2, last2, d_first);
}
```

```cpp
template<class InputIt1, class InputIt2,
         class OutputIt, class Compare>
OutputIt merge(InputIt1 first1, InputIt1 last1,
               InputIt2 first2, InputIt2 last2,
               OutputIt d_first, Compare comp)
{
    for (; first1 != last1; ++d_first)
    {
        if (first2 == last2)
            return std::copy(first1, last1, d_first);

        if (comp(*first2, *first1))
        {
            *d_first = *first2;
            ++first2;
        }
        else
        {
            *d_first = *first1;
            ++first1;
        }
    }
    return std::copy(first2, last2, d_first);
}
```

### Example

```cpp
#include <algorithm>
#include <functional>
#include <iostream>
#include <iterator>
#include <random>
#include <vector>

auto print = [](auto const rem, auto const& v)
{
    std::cout << rem;
    std::copy(v.begin(), v.end(), std::ostream_iterator<int>(std::cout, " "));
    std::cout << '\n';
};

int main()
{
    // fill the vectors with random numbers
    std::random_device rd;
    std::mt19937 mt(rd());
    std::uniform_int_distribution<> dis(0, 9);

    std::vector<int> v1(10), v2(10);
    std::generate(v1.begin(), v1.end(), std::bind(dis, std::ref(mt)));
    std::generate(v2.begin(), v2.end(), std::bind(dis, std::ref(mt)));

    print("Originally:\nv1: ", v1);
    print("v2: ", v2);

    std::sort(v1.begin(), v1.end());
    std::sort(v2.begin(), v2.end());

    print("After sorting:\nv1: ", v1);
    print("v2: ", v2);

    // merge
    std::vector<int> dst;
    std::merge(v1.begin(), v1.end(), v2.begin(), v2.end(), std::back_inserter(dst));

    print("After merging:\ndst: ", dst);
}
```

Possible output:

```text
Originally:
v1: 2 6 5 7 4 2 2 6 7 0
v2: 8 3 2 5 0 1 9 6 5 0
After sorting:
v1: 0 2 2 2 4 5 6 6 7 7
v2: 0 0 1 2 3 5 5 6 8 9
After merging:
dst: 0 0 0 1 2 2 2 2 3 4 5 5 5 6 6 6 7 7 8 9
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 780 | C++98 | the merge operation was not defined | defined

### See also

- **inplace_merge** — merges two ordered ranges in-place (function template)
- **is_sorted (C++11)** — checks whether a range is sorted into ascending order
  (function template)
- **set_union** — computes the union of two sets (function template)
- **sort** — sorts a range into ascending order (function template)
- **stable_sort** — sorts a range of elements while preserving order between
  equal elements (function template)
- **ranges::merge (C++20)** — merges two sorted ranges (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/merge*
