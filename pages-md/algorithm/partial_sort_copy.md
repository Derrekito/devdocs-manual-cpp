# std::partial_sort_copy

```cpp
template< class InputIt, class RandomIt >
RandomIt partial_sort_copy( InputIt first, InputIt last,
                            RandomIt d_first, RandomIt d_last );  // (until C++20)
template< class InputIt, class RandomIt >
constexpr RandomIt partial_sort_copy( InputIt first, InputIt last,
                                      RandomIt d_first, RandomIt d_last );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class RandomIt >
RandomIt partial_sort_copy( ExecutionPolicy&& policy, ForwardIt first, ForwardIt last,
                            RandomIt d_first, RandomIt d_last );  // (2) (since C++17)
template< class InputIt, class RandomIt, class Compare >
RandomIt partial_sort_copy( InputIt first, InputIt last,
                            RandomIt d_first, RandomIt d_last,
                            Compare comp );  // (until C++20)
template< class InputIt, class RandomIt, class Compare >
constexpr RandomIt partial_sort_copy( InputIt first, InputIt last,
                                      RandomIt d_first, RandomIt d_last,
                                      Compare comp );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class RandomIt, class Compare >
RandomIt partial_sort_copy( ExecutionPolicy&& policy, ForwardIt first, ForwardIt last,
                            RandomIt d_first, RandomIt d_last,
                            Compare comp );  // (4) (since C++17)
```

Sorts some of the elements in the range `[``first``,``last``)` in ascending
order, storing the result in the range `[``d_first``,``d_last``)`.

At most `d_last - d_first` of the elements are placed sorted to the range
`[``d_first``,``d_first + n``)`. `n` is the number of elements to sort (`n =
min(last - first, d_last - d_first)`). The order of equal elements is not
guaranteed to be preserved.

1) Elements are compared using `operator<`.

3) Elements are compared using the given binary comparison function `comp`.

2,4) Same as (1,3), but executed according to `policy`. These overloads do not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

### Parameters

- **first, last** — the range of elements to sort
- **d_first, d_last** — random access iterators defining the destination range
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
  such that an object of type RandomIt can be dereferenced and then implicitly
  converted to both of them. ​

**Type requirements**

**-`InputIt` must meet the requirements of LegacyInputIterator.**

**-`ForwardIt` must meet the requirements of LegacyForwardIterator.**

**-`RandomIt` must meet the requirements of ValueSwappable and LegacyRandomAccessIterator.**

**-The type of dereferenced `RandomIt` must meet the requirements of MoveAssignable and MoveConstructible.**

### Return value

An iterator to the element defining the upper boundary of the sorted range, i.e.
`d_first + min(last - first, d_last - d_first)`.

### Complexity

O(N·log(min(D,N)), where `N = std::distance(first, last)`, `D =
std::distance(d_first, d_last)` applications of `cmp`.

### Exceptions

The overloads with a template parameter named `ExecutionPolicy` report errors as
follows:

- If execution of a function invoked as part of the algorithm throws an
  exception and `ExecutionPolicy` is one of the standard policies,
  `std::terminate` is called. For any other `ExecutionPolicy`, the behavior is
  implementation-defined.
- If the algorithm fails to allocate memory, `std::bad_alloc` is thrown.

### Possible implementation

See also the implementations in libstdc++ and libc++.

### Example

The following code sorts a vector of integers and copies them into a smaller and
a larger vector.

```cpp
#include <algorithm>
#include <functional>
#include <iostream>
#include <string_view>
#include <type_traits>
#include <vector>

void println(std::string_view rem, auto const& v)
{
    std::cout << rem;
    if constexpr (std::is_scalar_v<std::decay_t<decltype(v)>>)
        std::cout << v;
    else
        for (int e : v)
            std::cout << e << ' ';
    std::cout << '\n';
}

int main()
{
    const auto v0 = {4, 2, 5, 1, 3};
    std::vector<int> v1 {10, 11, 12};
    std::vector<int> v2 {10, 11, 12, 13, 14, 15, 16};
    std::vector<int>::iterator it;

    it = std::partial_sort_copy(v0.begin(), v0.end(), v1.begin(), v1.end());
    println("Writing to the smaller vector in ascending order gives: ", v1);

    if (it == v1.end())
        println("The return value is the end iterator", ' ');

    it = std::partial_sort_copy(v0.begin(), v0.end(), v2.begin(), v2.end(),
                                std::greater<int>());

    println("Writing to the larger vector in descending order gives: ", v2);
    println("The return value is the iterator to ", *it);
}
```

Output:

```text
Writing to the smaller vector in ascending order gives: 1 2 3
The return value is the end iterator
Writing to the larger vector in descending order gives: 5 4 3 2 1 15 16
The return value is the iterator to 15
```

### See also

- **partial_sort** — sorts the first N elements of a range (function template)
- **sort** — sorts a range into ascending order (function template)
- **stable_sort** — sorts a range of elements while preserving order between
  equal elements (function template)
- **ranges::partial_sort_copy (C++20)** — copies and partially sorts a range of
  elements (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/partial_sort_copy*
