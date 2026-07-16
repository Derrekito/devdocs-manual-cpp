# std::minmax_element

```cpp
template< class ForwardIt >
std::pair<ForwardIt, ForwardIt>
    minmax_element( ForwardIt first, ForwardIt last );  // (since C++11) (until C++17)
template< class ForwardIt >
constexpr std::pair<ForwardIt, ForwardIt>
    minmax_element( ForwardIt first, ForwardIt last );  // (since C++17)
template< class ExecutionPolicy, class ForwardIt >
std::pair<ForwardIt, ForwardIt>
    minmax_element( ExecutionPolicy&& policy, ForwardIt first, ForwardIt last );  // (2) (since C++17)
template< class ForwardIt, class Compare >
std::pair<ForwardIt, ForwardIt>
    minmax_element( ForwardIt first, ForwardIt last, Compare comp );  // (since C++11) (until C++17)
template< class ForwardIt, class Compare >
constexpr std::pair<ForwardIt, ForwardIt>
    minmax_element( ForwardIt first, ForwardIt last, Compare comp );  // (since C++17)
template< class ExecutionPolicy, class ForwardIt, class Compare >
std::pair<ForwardIt, ForwardIt>
    minmax_element( ExecutionPolicy&& policy,
                    ForwardIt first, ForwardIt last, Compare comp );  // (4) (since C++17)
```

Finds the smallest and greatest element in the range `[``first``,``last``)`.

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
- **cmp** — comparison function object (i.e. an object that satisfies the
  requirements of Compare) which returns `true` if `*a` is *less* than `*b`. The
  signature of the comparison function should be equivalent to the following:
  `bool cmp(const Type1& a, const Type2& b);` While the signature does not need
  to have `const&`, the function must not modify the objects passed to it and
  must be able to accept all values of type (possibly const) `Type1` and `Type2`
  regardless of value category (thus, `Type1&` is not allowed, nor is `Type1`
  unless for `Type1` a move is equivalent to a copy(since C++11)). The types
  Type1 and Type2 must be such that an object of type ForwardIt can be
  dereferenced and then implicitly converted to both of them.

**Type requirements**

**-`ForwardIt` must meet the requirements of LegacyForwardIterator.**

### Return value

a pair consisting of an iterator to the smallest element as the first element
and an iterator to the greatest element as the second. Returns
`std::make_pair(first, first)` if the range is empty. If several elements are
equivalent to the smallest element, the iterator to the first such element is
returned. If several elements are equivalent to the largest element, the
iterator to the last such element is returned.

### Complexity

At most max(floor((3/2)*(N−1)), 0) applications of the predicate, where `N =
std::distance(first, last)`.

### Exceptions

The overloads with a template parameter named `ExecutionPolicy` report errors as
follows:

- If execution of a function invoked as part of the algorithm throws an
  exception and `ExecutionPolicy` is one of the standard policies,
  `std::terminate` is called. For any other `ExecutionPolicy`, the behavior is
  implementation-defined.
- If the algorithm fails to allocate memory, `std::bad_alloc` is thrown.

### Notes

This algorithm is different from `std::make_pair(std::min_element(),
std::max_element())`, not only in efficiency, but also in that this algorithm
finds the *last* biggest element while `std::max_element` finds the *first*
biggest element.

### Possible implementation

```cpp
template<class ForwardIt>
std::pair<ForwardIt, ForwardIt>
    minmax_element(ForwardIt first, ForwardIt last)
{
    using value_type = typename std::iterator_traits<ForwardIt>::value_type;
    return std::minmax_element(first, last, std::less<value_type>());
}
```

```cpp
template<class ForwardIt, class Compare>
std::pair<ForwardIt, ForwardIt>
    minmax_element(ForwardIt first, ForwardIt last, Compare comp)
{
    auto min = first, max = first;

    if (first == last || ++first == last)
        return {min, max};

    if (comp(*first, *min))
        min = first;
    else
        max = first;

    while (++first != last)
    {
        auto i = first;
        if (++first == last)
        {
            if (comp(*i, *min))
                min = i;
            else if (!(comp(*i, *max)))
                max = i;
            break;
        }
        else
        {
            if (comp(*first, *i))
            {
                if (comp(*first, *min))
                    min = first;
                if (!(comp(*i, *max)))
                    max = i;
            }
            else
            {
                if (comp(*i, *min))
                    min = i;
                if (!(comp(*first, *max)))
                    max = first;
            }
        }
    }
    return {min, max};
}
```

### Example

```cpp
#include <algorithm>
#include <iostream>

int main()
{
    const auto v = {3, 9, 1, 4, 2, 5, 9};
    const auto [min, max] = std::minmax_element(begin(v), end(v));

    std::cout << "min = " << *min << ", max = " << *max << '\n';
}
```

Output:

```text
min = 1, max = 9
```

### See also

- **min_element** — returns the smallest element in a range (function template)
- **max_element** — returns the largest element in a range (function template)
- **ranges::minmax_element (C++20)** — returns the smallest and the largest
  elements in a range (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/minmax_element*
