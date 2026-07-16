# std::max_element

```cpp
template< class ForwardIt >
ForwardIt max_element( ForwardIt first, ForwardIt last );  // (until C++17)
template< class ForwardIt >
constexpr ForwardIt max_element( ForwardIt first, ForwardIt last );  // (since C++17)
template< class ExecutionPolicy, class ForwardIt >
ForwardIt max_element( ExecutionPolicy&& policy,
                       ForwardIt first, ForwardIt last );  // (2) (since C++17)
template< class ForwardIt, class Compare >
ForwardIt max_element( ForwardIt first, ForwardIt last, Compare comp );  // (until C++17)
template< class ForwardIt, class Compare >
constexpr ForwardIt max_element( ForwardIt first, ForwardIt last,
                                 Compare comp );  // (since C++17)
template< class ExecutionPolicy, class ForwardIt, class Compare >
ForwardIt max_element( ExecutionPolicy&& policy,
                       ForwardIt first, ForwardIt last, Compare comp );  // (4) (since C++17)
```

Finds the greatest element in the range `[``first``,``last``)`.

1) Elements are compared using `operator<`.

3) Elements are compared using the given binary comparison function `comp`.

2,4) Same as (1,3), but executed according to `policy`. These overloads do not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

### Parameters

- **first, last** — forward iterators defining the range to examine
- **policy** — the execution policy to use. See execution policy for details.
- **comp** — comparison function object (i.e. an object that satisfies the
  requirements of Compare) which returns `true` if the first argument is *less*
  than the second. The signature of the comparison function should be equivalent
  to the following: `bool cmp(const Type1& a, const Type2& b);` While the
  signature does not need to have `const&`, the function must not modify the
  objects passed to it and must be able to accept all values of type (possibly
  const) `Type1` and `Type2` regardless of value category (thus, `Type1&` is not
  allowed, nor is `Type1` unless for `Type1` a move is equivalent to a
  copy(since C++11)). The types Type1 and Type2 must be such that an object of
  type ForwardIt can be dereferenced and then implicitly converted to both of
  them.

**Type requirements**

**-`ForwardIt` must meet the requirements of LegacyForwardIterator.**

### Return value

Iterator to the greatest element in the range `[``first``,``last``)`. If several
elements in the range are equivalent to the greatest element, returns the
iterator to the first such element. Returns `last` if the range is empty.

### Complexity

Exactly max(N-1,0) comparisons, where `N = std::distance(first, last)`.

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
template<class ForwardIt>
ForwardIt max_element(ForwardIt first, ForwardIt last)
{
    if (first == last)
        return last;

    ForwardIt largest = first;
    ++first;

    for (; first != last; ++first)
        if (*largest < *first)
            largest = first;

    return largest;
}
```

```cpp
template<class ForwardIt, class Compare>
ForwardIt max_element(ForwardIt first, ForwardIt last, Compare comp)
{
    if (first == last)
        return last;

    ForwardIt largest = first;
    ++first;

    for (; first != last; ++first)
        if (comp(*largest, *first))
            largest = first;

    return largest;
}
```

### Example

```cpp
#include <algorithm>
#include <cmath>
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> v {3, 1, -14, 1, 5, 9};
    std::vector<int>::iterator result;

    result = std::max_element(v.begin(), v.end());
    std::cout << "max element found at index "
              << std::distance(v.begin(), result)
              << " has value " << *result << '\n';

    result = std::max_element(v.begin(), v.end(), [](int a, int b)
    {
        return std::abs(a) < std::abs(b);
    });
    std::cout << "absolute max element found at index "
              << std::distance(v.begin(), result)
              << " has value " << *result << '\n';
}
```

Output:

```text
max element found at index 5 has value 9
absolute max element found at index 2 has value -14
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 212 | C++98 | the return value was not specified if `first == last` |
      returns `last` in this case

### See also

- **min_element** — returns the smallest element in a range (function template)
- **minmax_element (C++11)** — returns the smallest and the largest elements in
  a range (function template)
- **max** — returns the greater of the given values (function template)
- **ranges::max_element (C++20)** — returns the largest element in a range
  (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/max_element*
