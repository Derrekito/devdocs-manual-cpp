# std::mismatch

```cpp
template< class InputIt1, class InputIt2 >
std::pair<InputIt1, InputIt2>
    mismatch( InputIt1 first1, InputIt1 last1,
              InputIt2 first2 );  // (until C++20)
template< class InputIt1, class InputIt2 >
constexpr std::pair<InputIt1, InputIt2>
              mismatch( InputIt1 first1, InputIt1 last1,
                        InputIt2 first2 );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2 >
std::pair<ForwardIt1, ForwardIt2>
    mismatch( ExecutionPolicy&& policy, ForwardIt1 first1, ForwardIt1 last1,
              ForwardIt2 first2 );  // (2) (since C++17)
template< class InputIt1, class InputIt2, class BinaryPredicate >
std::pair<InputIt1, InputIt2>
    mismatch( InputIt1 first1, InputIt1 last1,
              InputIt2 first2,
              BinaryPredicate p );  // (until C++20)
template< class InputIt1, class InputIt2, class BinaryPredicate >
constexpr std::pair<InputIt1, InputIt2>
              mismatch( InputIt1 first1, InputIt1 last1,
                        InputIt2 first2,
                        BinaryPredicate p );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2,
          class BinaryPredicate >
std::pair<ForwardIt1, ForwardIt2>
    mismatch( ExecutionPolicy&& policy, ForwardIt1 first1, ForwardIt1 last1,
              ForwardIt2 first2,
              BinaryPredicate p );  // (4) (since C++17)
template< class InputIt1, class InputIt2 >
std::pair<InputIt1, InputIt2>
    mismatch( InputIt1 first1, InputIt1 last1,
              InputIt2 first2, InputIt2 last2 );  // (since C++14) (until C++20)
template< class InputIt1, class InputIt2 >
constexpr std::pair<InputIt1, InputIt2>
              mismatch( InputIt1 first1, InputIt1 last1,
                        InputIt2 first2, InputIt2 last2 );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2 >
std::pair<ForwardIt1, ForwardIt2>
    mismatch( ExecutionPolicy&& policy, ForwardIt1 first1, ForwardIt1 last1,
              ForwardIt2 first2, ForwardIt2 last2 );  // (6) (since C++17)
template< class InputIt1, class InputIt2, class BinaryPredicate >
std::pair<InputIt1, InputIt2>
    mismatch( InputIt1 first1, InputIt1 last1,
              InputIt2 first2, InputIt2 last2,
              BinaryPredicate p );  // (since C++14) (until C++20)
template< class InputIt1, class InputIt2, class BinaryPredicate >
constexpr std::pair<InputIt1, InputIt2>
              mismatch( InputIt1 first1, InputIt1 last1,
                        InputIt2 first2, InputIt2 last2,
                        BinaryPredicate p );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2,
          class BinaryPredicate >
std::pair<ForwardIt1, ForwardIt2>
    mismatch( ExecutionPolicy&& policy, ForwardIt1 first1, ForwardIt1 last1,
              ForwardIt2 first2, ForwardIt2 last2,
              BinaryPredicate p );  // (8) (since C++17)
```

Returns the first mismatching pair of elements from two ranges: one defined by
`[``first1``,``last1``)` and another defined by `[``first2``,``last2``)`. If
`last2` is not provided (overloads (1-4)), it denotes `first2 + (last1 -
first1)`.

1,5) Elements are compared using `operator==`.

3,7) Elements are compared using the given binary predicate `p`.

2,4,6,8) Same as (1,3,5,7), but executed according to `policy`. These overloads
   do not participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

### Parameters

- **first1, last1** — the first range of the elements
- **first2, last2** — the second range of the elements
- **policy** — the execution policy to use. See execution policy for details.
- **p** — binary predicate which returns ​`true` if the elements should be
  treated as equal. The signature of the predicate function should be equivalent
  to the following: `bool pred(const Type1 &a, const Type2 &b);` While the
  signature does not need to have `const &`, the function must not modify the
  objects passed to it and must be able to accept all values of type (possibly
  const) `Type1` and `Type2` regardless of value category (thus, `Type1 &` is
  not allowed, nor is `Type1` unless for `Type1` a move is equivalent to a
  copy(since C++11)). The types Type1 and Type2 must be such that objects of
  types InputIt1 and InputIt2 can be dereferenced and then implicitly converted
  to Type1 and Type2 respectively. ​

**Type requirements**

**-`InputIt1` must meet the requirements of LegacyInputIterator.**

**-`InputIt2` must meet the requirements of LegacyInputIterator.**

**-`ForwardIt1` must meet the requirements of LegacyForwardIterator.**

**-`ForwardIt2` must meet the requirements of LegacyForwardIterator.**

**-`BinaryPredicate` must meet the requirements of BinaryPredicate.**

### Return value

`std::pair` with iterators to the first two non-equal elements.

If no mismatches are found when the comparison reaches `last1`, the pair holds
`last1` and the corresponding iterator from the second range. The behavior is
undefined if the second range is shorter than the first range.
*(until C++14)*

If no mismatches are found when the comparison reaches `last1` or `last2`,
whichever happens first, the pair holds the end iterator and the corresponding
iterator from the other range.
*(since C++14)*

### Complexity

1-4) At most `last1 - first1` applications of `operator==` or the predicate `p`

5-8) At most `std::min(last1 - first1, last2 - first2)` applications of
   `operator==` or the predicate `p`.

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
template<class InputIt1, class InputIt2>
std::pair<InputIt1, InputIt2>
    mismatch(InputIt1 first1, InputIt1 last1, InputIt2 first2)
{
    while (first1 != last1 && *first1 == *first2)
        ++first1, ++first2;

    return std::make_pair(first1, first2);
}
```

```cpp
template<class InputIt1, class InputIt2, class BinaryPredicate>
std::pair<InputIt1, InputIt2>
    mismatch(InputIt1 first1, InputIt1 last1, InputIt2 first2, BinaryPredicate p)
{
    while (first1 != last1 && p(*first1, *first2))
        ++first1, ++first2;

    return std::make_pair(first1, first2);
}
```

```cpp
template<class InputIt1, class InputIt2>
std::pair<InputIt1, InputIt2>
    mismatch(InputIt1 first1, InputIt1 last1, InputIt2 first2, InputIt2 last2)
{
    while (first1 != last1 && first2 != last2 && *first1 == *first2)
        ++first1, ++first2;

    return std::make_pair(first1, first2);
}
```

```cpp
template<class InputIt1, class InputIt2, class BinaryPredicate>
std::pair<InputIt1, InputIt2>
    mismatch(InputIt1 first1, InputIt1 last1,
             InputIt2 first2, InputIt2 last2, BinaryPredicate p)
{
    while (first1 != last1 && first2 != last2 && p(*first1, *first2))
        ++first1, ++first2;

    return std::make_pair(first1, first2);
}
```

### Example

This program determines the longest substring that is simultaneously found at
the very beginning of the given string and at the very end of it, in reverse
order (possibly overlapping).

```cpp
#include <algorithm>
#include <iostream>
#include <string>

std::string mirror_ends(const std::string& in)
{
    return std::string(in.begin(),
                       std::mismatch(in.begin(), in.end(), in.rbegin()).first);
}

int main()
{
    std::cout << mirror_ends("abXYZba") << '\n'
              << mirror_ends("abca") << '\n'
              << mirror_ends("aba") << '\n';
}
```

Output:

```text
ab
a
aba
```

### See also

- **equal** — determines if two sets of elements are the same (function
  template)
- **findfind_iffind_if_not (C++11)** — finds the first element satisfying
  specific criteria (function template)
- **lexicographical_compare** — returns `true` if one range is lexicographically
  less than another (function template)
- **search** — searches for a range of elements (function template)
- **ranges::mismatch (C++20)** — finds the first position where two ranges
  differ (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/mismatch*
