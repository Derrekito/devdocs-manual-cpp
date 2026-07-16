# std::equal

```cpp
template< class InputIt1, class InputIt2 >
bool equal( InputIt1 first1, InputIt1 last1,
            InputIt2 first2 );  // (until C++20)
template< class InputIt1, class InputIt2 >
constexpr bool equal( InputIt1 first1, InputIt1 last1,
                      InputIt2 first2 );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2 >
bool equal( ExecutionPolicy&& policy,
            ForwardIt1 first1, ForwardIt1 last1,
            ForwardIt2 first2 );  // (2) (since C++17)
template< class InputIt1, class InputIt2, class BinaryPredicate >
bool equal( InputIt1 first1, InputIt1 last1,
            InputIt2 first2,
            BinaryPredicate p );  // (until C++20)
template< class InputIt1, class InputIt2, class BinaryPredicate >
constexpr bool equal( InputIt1 first1, InputIt1 last1,
                      InputIt2 first2,
                      BinaryPredicate p );  // (since C++20)
template< class ExecutionPolicy,
          class ForwardIt1, class ForwardIt2, class BinaryPredicate >
bool equal( ExecutionPolicy&& policy,
            ForwardIt1 first1, ForwardIt1 last1,
            ForwardIt2 first2,
            BinaryPredicate p );  // (4) (since C++17)
template< class InputIt1, class InputIt2 >
bool equal( InputIt1 first1, InputIt1 last1,
            InputIt2 first2, InputIt2 last2 );  // (since C++14) (until C++20)
template< class InputIt1, class InputIt2 >
constexpr bool equal( InputIt1 first1, InputIt1 last1,
                      InputIt2 first2, InputIt2 last2 );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2 >
bool equal( ExecutionPolicy&& policy,
            ForwardIt1 first1, ForwardIt1 last1,
            ForwardIt2 first2, ForwardIt2 last2 );  // (6) (since C++17)
template< class InputIt1, class InputIt2, class BinaryPredicate >
bool equal( InputIt1 first1, InputIt1 last1,
            InputIt2 first2, InputIt2 last2,
            BinaryPredicate p );  // (since C++14) (until C++20)
template< class InputIt1, class InputIt2, class BinaryPredicate >
constexpr bool equal( InputIt1 first1, InputIt1 last1,
                      InputIt2 first2, InputIt2 last2,
                      BinaryPredicate p );  // (since C++20)
template< class ExecutionPolicy,
          class ForwardIt1, class ForwardIt2, class BinaryPredicate >
bool equal( ExecutionPolicy&& policy,
            ForwardIt1 first1, ForwardIt1 last1,
            ForwardIt2 first2, ForwardIt2 last2,
            BinaryPredicate p );  // (8) (since C++17)
```

1,3) Returns `true` if the range `[``first1``,``last1``)` is equal to the range
   `[``first2``,``first2 + (last1 - first1)``)`, and `false` otherwise.

5,7) Returns `true` if the range `[``first1``,``last1``)` is equal to the range
   `[``first2``,``last2``)`, and `false` otherwise.

2,4,6,8) Same as (1,3,5,7), but executed according to `policy`. These overloads
   do not participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

Two ranges are considered equal if they have the same number of elements and,
for every iterator `i` in the range `[``first1``,``last1``)`, `*i` equals
`*(first2 + (i - first1))`. The overloads (1,2,5,6) use `operator==` to
determine if two elements are equal, whereas overloads (3,4,7,8) use the given
binary predicate `p`.

### Parameters

- **first1, last1** — the first range of the elements to compare
- **first2, last2** — the second range of the elements to compare
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

**-`InputIt1, InputIt2` must meet the requirements of LegacyInputIterator.**

**-`ForwardIt1, ForwardIt2` must meet the requirements of LegacyForwardIterator.**

### Return value

5-8) If the length of the range `[``first1``,``last1``)` does not equal the
   length of the range `[``first2``,``last2``)`, returns `false`.

If the elements in the two ranges are equal, returns `true`.

Otherwise returns `false`.

### Notes

`std::equal` should not be used to compare the ranges formed by the iterators
from `std::unordered_set`, `std::unordered_multiset`, `std::unordered_map`, or
`std::unordered_multimap` because the order in which the elements are stored in
those containers may be different even if the two containers store the same
elements.

When comparing entire containers for equality, `operator==` for the
corresponding container are usually preferred.

### Complexity

1,3) At most `last1` - `first1` applications of the predicate.

5,7) At most min(`last1` - `first1`, `last2` - `first2`) applications of the
   predicate. However, if `InputIt1` and `InputIt2` meet the requirements of
   LegacyRandomAccessIterator and `last1 - first1 != last2 - first2` then no
   applications of the predicate are made (size mismatch is detected without
   looking at any elements).

2,4,6,8) Same, but the complexity is specified as O(x), rather than "at most x".

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
constexpr //< since C++20
bool equal(InputIt1 first1, InputIt1 last1, InputIt2 first2)
{
    for (; first1 != last1; ++first1, ++first2)
        if (!(*first1 == *first2))
            return false;

    return true;
}
```

```cpp
template<class InputIt1, class InputIt2, class BinaryPredicate>
constexpr //< since C++20
bool equal(InputIt1 first1, InputIt1 last1,
           InputIt2 first2, BinaryPredicate p)
{
    for (; first1 != last1; ++first1, ++first2)
        if (!p(*first1, *first2))
            return false;

    return true;
}
```

### Example

The following code uses `std::equal` to test if a string is a palindrome.

```cpp
#include <algorithm>
#include <iomanip>
#include <iostream>
#include <string_view>

constexpr bool is_palindrome(const std::string_view& s)
{
    return std::equal(s.cbegin(), s.cbegin() + s.size() / 2, s.crbegin());
}

void test(const std::string_view& s)
{
    std::cout << std::quoted(s)
              << (is_palindrome(s) ? " is" : " is not")
              << " a palindrome\n";
}

int main()
{
    test("radar");
    test("hello");
}
```

Output:

```text
"radar" is a palindrome
"hello" is not a palindrome
```

### See also

- **findfind_iffind_if_not (C++11)** — finds the first element satisfying
  specific criteria (function template)
- **lexicographical_compare** — returns `true` if one range is lexicographically
  less than another (function template)
- **mismatch** — finds the first position where two ranges differ (function
  template)
- **search** — searches for a range of elements (function template)
- **ranges::equal (C++20)** — determines if two sets of elements are the same
  (niebloid)
- **equal_to** — function object implementing `x == y` (class template)
- **equal_range** — returns range of elements matching a specific key (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/equal*
