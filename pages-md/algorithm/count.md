# std::count, std::count_if

```cpp
template< class InputIt, class T >
typename iterator_traits<InputIt>::difference_type
    count( InputIt first, InputIt last, const T& value );  // (until C++20)
template< class InputIt, class T >
constexpr typename iterator_traits<InputIt>::difference_type
    count( InputIt first, InputIt last, const T& value );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class T >
typename iterator_traits<ForwardIt>::difference_type
    count( ExecutionPolicy&& policy,
           ForwardIt first, ForwardIt last, const T& value );  // (2) (since C++17)
template< class InputIt, class UnaryPredicate >
typename iterator_traits<InputIt>::difference_type
    count_if( InputIt first, InputIt last, UnaryPredicate p );  // (until C++20)
template< class InputIt, class UnaryPredicate >
constexpr typename iterator_traits<InputIt>::difference_type
    count_if( InputIt first, InputIt last, UnaryPredicate p );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class UnaryPredicate >
typename iterator_traits<ForwardIt>::difference_type
    count_if( ExecutionPolicy&& policy,
              ForwardIt first, ForwardIt last, UnaryPredicate p );  // (4) (since C++17)
```

Returns the number of elements in the range `[``first``,``last``)` satisfying
specific criteria.

1) Counts the elements that are equal to `value` (using `operator==`).

3) Counts elements for which predicate `p` returns `true`.

2,4) Same as (1,3), but executed according to `policy`. These overloads do not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

### Parameters

- **first, last** — the range of elements to examine
- **value** — the value to search for
- **policy** — the execution policy to use. See execution policy for details.
- **p** — unary predicate which returns ​`true` for the required elements. The
  expression `p(v)` must be convertible to `bool` for every argument `v` of type
  (possibly const) `VT`, where `VT` is the value type of `InputIt`, regardless
  of value category, and must not modify `v`. Thus, a parameter type of `VT&`is
  not allowed, nor is `VT` unless for `VT` a move is equivalent to a copy(since
  C++11). ​

**Type requirements**

**-`InputIt` must meet the requirements of LegacyInputIterator.**

**-`ForwardIt` must meet the requirements of LegacyForwardIterator.**

### Return value

The number of iterators `it` in the range `[``first``,``last``)` satisfying the
following condition:

1,2) `*it == value` is `true`.

3,4) `p(*it) != false` is `true`.

### Complexity

Given `N` as `std::distance(first, last)`:

1,2) Exactly `N` comparisons with `value` using `operator==`.

3,4) Exactly `N` applications of the predicate `p`.

### Exceptions

The overloads with a template parameter named `ExecutionPolicy` report errors as
follows:

- If execution of a function invoked as part of the algorithm throws an
  exception and `ExecutionPolicy` is one of the standard policies,
  `std::terminate` is called. For any other `ExecutionPolicy`, the behavior is
  implementation-defined.
- If the algorithm fails to allocate memory, `std::bad_alloc` is thrown.

### Notes

For the number of elements in the range `[``first``,``last``)` without any
additional criteria, see `std::distance`.

### Possible implementation

See also the implementations of `count` in libstdc++ and libc++.

See also the implementations of `count_if` in libstdc++ and libc++.

```cpp
template<class InputIt, class T>
typename iterator_traits<InputIt>::difference_type
    count(InputIt first, InputIt last, const T& value)
{
    typename iterator_traits<InputIt>::difference_type ret = 0;
    for (; first != last; ++first)
        if (*first == value)
            ++ret;
    return ret;
}
```

```cpp
template<class InputIt, class UnaryPredicate>
typename iterator_traits<InputIt>::difference_type
    count_if(InputIt first, InputIt last, UnaryPredicate p)
{
    typename iterator_traits<InputIt>::difference_type ret = 0;
    for (; first != last; ++first)
        if (p(*first))
            ++ret;
    return ret;
}
```

### Example

```cpp
#include <algorithm>
#include <array>
#include <iostream>
#include <iterator>

int main()
{
    constexpr std::array v{1, 2, 3, 4, 4, 3, 7, 8, 9, 10};
    std::cout << "v: ";
    std::copy(v.cbegin(), v.cend(), std::ostream_iterator<int>(std::cout, " "));
    std::cout << '\n';

    // Determine how many integers match a target value.
    for (const int target : {3, 4, 5})
    {
        const int num_items = std::count(v.cbegin(), v.cend(), target);
        std::cout << "number: " << target << ", count: " << num_items << '\n';
    }

    // Use a lambda expression to count elements divisible by 4.
    int count_div4 = std::count_if(v.begin(), v.end(), [](int i) { return i % 4 == 0; });
    std::cout << "numbers divisible by four: " << count_div4 << '\n';

    // A simplified version of `distance` with O(N) complexity:
    auto distance = [](auto first, auto last)
    {
        return std::count_if(first, last, [](auto) { return true; });
    };
    static_assert(distance(v.begin(), v.end()) == 10);
}
```

Output:

```text
v: 1 2 3 4 4 3 7 8 9 10
number: 3, count: 2
number: 4, count: 2
number: 5, count: 0
numbers divisible by four: 3
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 283 | C++98 | `T` was required to be EqualityComparable, but the value
      type of `InputIt` is not always `T` | removed the requirement

### See also

- **distance** — returns the distance between two iterators (function template)
- **ranges::countranges::count_if (C++20)(C++20)** — returns the number of
  elements satisfying specific criteria (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/count*
