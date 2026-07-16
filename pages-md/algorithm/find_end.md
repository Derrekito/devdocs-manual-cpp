# std::find_end

```cpp
template< class ForwardIt1, class ForwardIt2 >
ForwardIt1 find_end( ForwardIt1 first, ForwardIt1 last,
                     ForwardIt2 s_first, ForwardIt2 s_last );  // (until C++20)
template< class ForwardIt1, class ForwardIt2 >
constexpr ForwardIt1 find_end( ForwardIt1 first, ForwardIt1 last,
                               ForwardIt2 s_first, ForwardIt2 s_last );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2 >
ForwardIt1 find_end( ExecutionPolicy&& policy,
                     ForwardIt1 first, ForwardIt1 last,
                     ForwardIt2 s_first, ForwardIt2 s_last );  // (2) (since C++17)
template< class ForwardIt1, class ForwardIt2, class BinaryPredicate >
ForwardIt1 find_end( ForwardIt1 first, ForwardIt1 last,
                     ForwardIt2 s_first, ForwardIt2 s_last,
                     BinaryPredicate p );  // (until C++20)
template< class ForwardIt1, class ForwardIt2, class BinaryPredicate >
constexpr ForwardIt1 find_end( ForwardIt1 first, ForwardIt1 last,
                               ForwardIt2 s_first, ForwardIt2 s_last,
                               BinaryPredicate p );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2,
          class BinaryPredicate >
ForwardIt1 find_end( ExecutionPolicy&& policy,
                     ForwardIt1 first, ForwardIt1 last,
                     ForwardIt2 s_first, ForwardIt2 s_last,
                     BinaryPredicate p );  // (4) (since C++17)
```

Searches for the last occurrence of the sequence `[``s_first``,``s_last``)` in
the range `[``first``,``last``)`.

1) Elements are compared using `operator==`.

3) Elements are compared using the given binary predicate `p`.

2,4) Same as (1,3), but executed according to `policy`. These overloads do not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

### Parameters

- **first, last** — the range of elements to examine
- **s_first, s_last** — the range of elements to search for
- **policy** — the execution policy to use. See execution policy for details.
- **p** — binary predicate which returns ​`true` if the elements should be
  treated as equal. The signature of the predicate function should be equivalent
  to the following: `bool pred(const Type1 &a, const Type2 &b);` While the
  signature does not need to have `const &`, the function must not modify the
  objects passed to it and must be able to accept all values of type (possibly
  const) `Type1` and `Type2` regardless of value category (thus, `Type1 &` is
  not allowed, nor is `Type1` unless for `Type1` a move is equivalent to a
  copy(since C++11)). The types Type1 and Type2 must be such that objects of
  types ForwardIt1 and ForwardIt2 can be dereferenced and then implicitly
  converted to Type1 and Type2 respectively. ​

**Type requirements**

**-`ForwardIt1` must meet the requirements of LegacyForwardIterator.**

**-`ForwardIt2` must meet the requirements of LegacyForwardIterator.**

### Return value

Iterator to the beginning of last occurrence of the sequence
`[``s_first``,``s_last``)` in range `[``first``,``last``)`.

If `[``s_first``,``s_last``)` is empty or if no such sequence is found, `last`
is returned.

### Complexity

Given \(\scriptsize N\)N as `std::distance(first1, last1)` and \(\scriptsize
S\)S as `std::distance(first2, last2)`:

1,2) At most \(\scriptsize S\cdot(N-S+1)\)S·(N-S+1) comparisons using
   `operator==`.

3,4) At most \(\scriptsize S\cdot(N-S+1)\)S·(N-S+1) applications of the
   predicate `p`.

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
template<class ForwardIt1, class ForwardIt2>
ForwardIt1 find_end(ForwardIt1 first, ForwardIt1 last,
                    ForwardIt2 s_first, ForwardIt2 s_last)
{
    if (s_first == s_last)
        return last;
    ForwardIt1 result = last;
    while (true)
    {
        ForwardIt1 new_result = std::search(first, last, s_first, s_last);
        if (new_result == last)
            break;
        else
        {
            result = new_result;
            first = result;
            ++first;
        }
    }
    return result;
}
```

```cpp
template<class ForwardIt1, class ForwardIt2, class BinaryPredicate>
ForwardIt1 find_end(ForwardIt1 first, ForwardIt1 last,
                    ForwardIt2 s_first, ForwardIt2 s_last,
                    BinaryPredicate p)
{
    if (s_first == s_last)
        return last;
    ForwardIt1 result = last;
    while (true)
    {
        ForwardIt1 new_result = std::search(first, last, s_first, s_last, p);
        if (new_result == last)
            break;
        else
        {
            result = new_result;
            first = result;
            ++first;
        }
    }
    return result;
}
```

### Example

```cpp
#include <algorithm>
#include <array>
#include <cmath>
#include <iostream>

auto print_result = [](auto result, const auto& v)
{
    result == v.end()
        ? std::cout << "Sequence not found\n"
        : std::cout << "Last occurrence is at: " << std::distance(v.begin(), result)
                    << '\n';
};

int main()
{
    const auto v = {1, 2, 3, 4, 1, 2, 3, 4, 1, 2, 3, 4};

    for (auto const& x : {std::array{1, 2, 3}, {4, 5, 6}})
    {
        auto iter = std::find_end(v.begin(), v.end(), x.begin(), x.end()); // overload (1)
        print_result(iter, v);
    }

    for (auto const& x : {std::array{-1, -2, -3}, {-4, -5, -6}})
    {
        auto iter = std::find_end(v.begin(), v.end(), x.begin(), x.end(), // overload (3)
                                  [](int x, int y)
                                  {
                                      return std::abs(x) == std::abs(y);
                                  });
        print_result(iter, v);
    }
}
```

Output:

```text
Last occurrence is at: 8
Sequence not found
Last occurrence is at: 8
Sequence not found
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 1205 | C++98 | the return value was unclear if `[``s_first``,``s_last``)`
      is empty | returns `last` in this case

### See also

- **search** — searches for a range of elements (function template)
- **includes** — returns `true` if one sequence is a subsequence of another
  (function template)
- **adjacent_find** — finds the first two adjacent items that are equal (or
  satisfy a given predicate) (function template)
- **findfind_iffind_if_not (C++11)** — finds the first element satisfying
  specific criteria (function template)
- **find_first_of** — searches for any one of a set of elements (function
  template)
- **search_n** — searches a range for a number of consecutive copies of an
  element (function template)
- **ranges::find_end (C++20)** — finds the last sequence of elements in a
  certain range (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/find_end*
