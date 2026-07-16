# std::find_first_of

```cpp
template< class InputIt, class ForwardIt >
InputIt find_first_of( InputIt first, InputIt last,
                       ForwardIt s_first, ForwardIt s_last );  // (until C++20)
template< class InputIt, class ForwardIt >
constexpr InputIt find_first_of( InputIt first, InputIt last,
                                 ForwardIt s_first, ForwardIt s_last );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2 >
ForwardIt1 find_first_of( ExecutionPolicy&& policy,
                          ForwardIt1 first, ForwardIt1 last,
                          ForwardIt2 s_first, ForwardIt2 s_last );  // (2) (since C++17)
template< class InputIt, class ForwardIt, class BinaryPredicate >
InputIt find_first_of( InputIt first, InputIt last,
                       ForwardIt s_first, ForwardIt s_last,
                       BinaryPredicate p );  // (until C++20)
template< class InputIt, class ForwardIt, class BinaryPredicate >
constexpr InputIt find_first_of( InputIt first, InputIt last,
                                 ForwardIt s_first, ForwardIt s_last,
                                 BinaryPredicate p );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2,
          class BinaryPredicate >
ForwardIt1 find_first_of( ExecutionPolicy&& policy,
                          ForwardIt1 first, ForwardIt last,
                          ForwardIt2 s_first, ForwardIt2 s_last,
                          BinaryPredicate p );  // (4) (since C++17)
```

Searches the range `[``first``,``last``)` for any of the elements in the range
`[``s_first``,``s_last``)`.

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

**-`InputIt` must meet the requirements of LegacyInputIterator.**

**-`ForwardIt` must meet the requirements of LegacyForwardIterator.**

**-`ForwardIt1` must meet the requirements of LegacyForwardIterator.**

**-`ForwardIt2` must meet the requirements of LegacyForwardIterator.**

**-`BinaryPredicate` must meet the requirements of BinaryPredicate.**

### Return value

Iterator to the first element in the range `[``first``,``last``)` that is equal
to an element from the range `[``s_first``,``s_last``)`.

If `[``s_first``,``s_last``)` is empty or if no such element is found, `last` is
returned.

### Complexity

Given \(\scriptsize N\)N as `std::distance(first, last)` and \(\scriptsize S\)S
as `std::distance(s_first, s_last)`:

1,2) At most \(\scriptsize N\cdot S\)N·S comparisons using `operator==`.

3,4) At most \(\scriptsize N\cdot S\)N·S applications of the predicate `p`.

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
template<class InputIt, class ForwardIt>
InputIt find_first_of(InputIt first, InputIt last,
                      ForwardIt s_first, ForwardIt s_last)
{
    for (; first != last; ++first)
        for (ForwardIt it = s_first; it != s_last; ++it)
            if (*first == *it)
                return first;
    return last;
}
```

```cpp
template<class InputIt, class ForwardIt, class BinaryPredicate>
InputIt find_first_of(InputIt first, InputIt last,
                      ForwardIt s_first, ForwardIt s_last,
                      BinaryPredicate p)
{
    for (; first != last; ++first)
        for (ForwardIt it = s_first; it != s_last; ++it)
            if (p(*first, *it))
                return first;
    return last;
}
```

### Example

The following code searches for any of specified integers in a vector of
integers:

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

auto print_sequence = [](auto const id, auto const& seq, int pos = -1)
{
    std::cout << id << "{ ";
    for (int i{}; auto const& e : seq)
    {
        const bool mark{i == pos};
        std::cout << (i++ ? ", " : "");
        std::cout << (mark ? "[ " : "") << e << (mark ? " ]" : "");
    }
    std::cout << " }\n";
};

int main()
{
    const std::vector<int> v{0, 2, 3, 25, 5};
    const auto t1 = {19, 10, 3, 4};
    const auto t2 = {1, 6, 7, 9};

    auto find_any_of = [](const auto& v, const auto& t)
    {
        const auto result = std::find_first_of(v.begin(), v.end(),
                                               t.begin(), t.end());
        if (result == v.end())
        {
            std::cout << "No elements of v are equal to any element of ";
            print_sequence("t = ", t);
            print_sequence("v = ", v);
        }
        else
        {
            const auto pos = std::distance(v.begin(), result);
            std::cout << "Found a match (" << *result << ") at position " << pos;
            print_sequence(", where t = ", t);
            print_sequence("v = ", v, pos);
        }
    };

    find_any_of(v, t1);
    find_any_of(v, t2);
}
```

Output:

```text
Found a match (3) at position 2, where t = { 19, 10, 3, 4 }
v = { 0, 2, [ 3 ], 25, 5 }
No elements of v are equal to any element of t = { 1, 6, 7, 9 }
v = { 0, 2, 3, 25, 5 }
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 576 | C++98 | `first` and `last` needed to be LegacyForwardIterators |
      they only need to be LegacyInputIterators
  LWG 1205 | C++98 | the return value was unclear if `[``s_first``,``s_last``)`
      is empty | returns `last` in this case

### See also

- **findfind_iffind_if_not (C++11)** — finds the first element satisfying
  specific criteria (function template)
- **ranges::find_first_of (C++20)** — searches for any one of a set of elements
  (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/find_first_of*
