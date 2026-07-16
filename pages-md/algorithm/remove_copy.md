# std::remove_copy, std::remove_copy_if

```cpp
template< class InputIt, class OutputIt, class T >
OutputIt remove_copy( InputIt first, InputIt last,
                      OutputIt d_first, const T& value );  // (until C++20)
template< class InputIt, class OutputIt, class T >
constexpr OutputIt remove_copy( InputIt first, InputIt last,
                                OutputIt d_first, const T& value );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2, class T >
ForwardIt2 remove_copy( ExecutionPolicy&& policy,
                        ForwardIt1 first, ForwardIt1 last,
                        ForwardIt2 d_first, const T& value );  // (2) (since C++17)
template< class InputIt, class OutputIt, class UnaryPredicate >
OutputIt remove_copy_if( InputIt first, InputIt last,
                         OutputIt d_first, UnaryPredicate p );  // (until C++20)
template< class InputIt, class OutputIt, class UnaryPredicate >
constexpr OutputIt remove_copy_if( InputIt first, InputIt last,
                                   OutputIt d_first, UnaryPredicate p );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1,
          class ForwardIt2, class UnaryPredicate >
ForwardIt2 remove_copy_if( ExecutionPolicy&& policy,
                           ForwardIt1 first, ForwardIt1 last,
                           ForwardIt2 d_first, UnaryPredicate p );  // (4) (since C++17)
```

Copies elements from the range `[``first``,``last``)`, to another range
beginning at `d_first`, omitting the elements which satisfy specific criteria.

1) Ignores all elements that are equal to `value`.

3) Ignores all elements for which predicate `p` returns `true`.

2,4) Same as (1,3), but executed according to `policy`. These overloads do not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

If source and destination ranges overlap, the behavior is undefined.

### Parameters

- **first, last** — the range of elements to copy
- **d_first** — the beginning of the destination range
- **value** — the value of the elements not to copy
- **policy** — the execution policy to use. See execution policy for details.

**Type requirements**

**-`InputIt` must meet the requirements of LegacyInputIterator.**

**-`OutputIt` must meet the requirements of LegacyOutputIterator.**

**-`ForwardIt1, ForwardIt2` must meet the requirements of LegacyForwardIterator.**

**-`UnaryPredicate` must meet the requirements of Predicate.**

The expression `*d_first = *first` must be valid.
*(until C++20)*

`*first` must be writable to `d_first`.
*(since C++20)*

### Return value

Iterator to the element past the last element copied.

### Complexity

Given `N` as `std::distance(first, last)`:

1,2) exactly `N` comparisons with `value` using `operator==`.

3,4) exactly `N` applications of the predicate `p`.

For the overloads with an ExecutionPolicy, there may be a performance cost if
`ForwardIt1`'s `value_type` is not MoveConstructible.

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
template<class InputIt, class OutputIt, class T>
OutputIt remove_copy(InputIt first, InputIt last, OutputIt d_first, const T& value)
{
    for (; first != last; ++first)
        if (!(*first == value))
            *d_first++ = *first;
    return d_first;
}
```

```cpp
template<class InputIt, class OutputIt, class UnaryPredicate>
OutputIt remove_copy_if(InputIt first, InputIt last, OutputIt d_first, UnaryPredicate p)
{
    for (; first != last; ++first)
        if (!p(*first))
            *d_first++ = *first;
    return d_first;
}
```

### Example

The following code outputs a string while erasing the hash characters '#' on the
fly.

```cpp
#include <algorithm>
#include <iomanip>
#include <iostream>
#include <iterator>
#include <string>

int main()
{
    std::string str = "#Return #Value #Optimization";
    std::cout << "before: " << std::quoted(str) << '\n';

    std::cout << "after:  \"";
    std::remove_copy(str.begin(), str.end(),
                     std::ostream_iterator<char>(std::cout), '#');
    std::cout << "\"\n";
}
```

Output:

```text
before: "#Return #Value #Optimization"
after:  "Return Value Optimization"
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 779 | C++98 | `T` was required to be EqualityComparable, but the value
      type of `ForwardIt` is not always `T` | required `*d_first = *first` to be
      valid instead

### See also

- **removeremove_if** — removes elements satisfying specific criteria (function
  template)
- **copycopy_if (C++11)** — copies a range of elements to a new location
  (function template)
- **partition_copy (C++11)** — copies a range dividing the elements into two
  groups (function template)
- **ranges::remove_copyranges::remove_copy_if (C++20)(C++20)** — copies a range
  of elements omitting those that satisfy specific criteria (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/remove_copy*
