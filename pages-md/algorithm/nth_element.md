# std::nth_element

```cpp
template< class RandomIt >
void nth_element( RandomIt first, RandomIt nth, RandomIt last );  // (1) (constexpr since C++20)
template< class ExecutionPolicy, class RandomIt >
void nth_element( ExecutionPolicy&& policy,
                  RandomIt first, RandomIt nth, RandomIt last );  // (2) (since C++17)
template< class RandomIt, class Compare >
void nth_element( RandomIt first, RandomIt nth, RandomIt last,
                  Compare comp );  // (3) (constexpr since C++20)
template< class ExecutionPolicy, class RandomIt, class Compare >
void nth_element( ExecutionPolicy&& policy,
                  RandomIt first, RandomIt nth, RandomIt last,
                  Compare comp );  // (4) (since C++17)
```

`nth_element` is a partial sorting algorithm that rearranges elements in
`[``first``,``last``)` such that:

- The element pointed at by `nth` is changed to whatever element would occur in
  that position if `[``first``,``last``)` were sorted.
- All of the elements before this new `nth` element are less than or equal to
  the elements after the new `nth` element.

More formally, `nth_element` partially sorts the range `[``first``,``last``)` in
ascending order so that for any `i` in the range
`[``first``,``nth``)` and for any `j` in the range `[``nth``,``last``)` the
following condition is met:

1,2) `!(*j < *i)`,

3,4) `comp(*j, *i) == false`.

The element placed in the `nth` position is exactly the element that would occur
in this position if the range was fully sorted.

`nth` may be the end iterator, in this case the function has no effect.

1) Elements are compared using `operator<`.

3) Elements are compared using the given binary comparison function `comp`.

2,4) Same as (1,3), but executed according to `policy`. These overloads do not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

### Parameters

- **first, last** — random access iterators defining the range sort
- **nth** — random access iterator defining the sort partition point
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

**-`RandomIt` must meet the requirements of ValueSwappable and LegacyRandomAccessIterator.**

**-The type of dereferenced `RandomIt` must meet the requirements of MoveAssignable and MoveConstructible.**

### Return value

(none)

### Complexity

1,3) Linear in `std::distance(first, last)` on average.

2,4) \(\scriptsize O(N)\)O(N) applications of the predicate, and \(\scriptsize
   O(N \log N)\)O(N \log N) swaps, where `N = last - first`.

### Exceptions

The overloads with a template parameter named `ExecutionPolicy` report errors as
follows:

- If execution of a function invoked as part of the algorithm throws an
  exception and `ExecutionPolicy` is one of the standard policies,
  `std::terminate` is called. For any other `ExecutionPolicy`, the behavior is
  implementation-defined.
- If the algorithm fails to allocate memory, `std::bad_alloc` is thrown.

### Notes

The algorithm used is typically Introselect although other Selection algorithm
with suitable average-case complexity are allowed.

### Possible implementation

See also the implementations in libstdc++, libc++, and msvc stl.

### Example

```cpp
#include <algorithm>
#include <cassert>
#include <functional>
#include <iostream>
#include <numeric>
#include <vector>

void printVec(const std::vector<int>& vec)
{
    std::cout << "v = {";
    for (char sep[]{0, ' ', 0}; const int i : vec)
        std::cout << sep << i, sep[0] = ',';
    std::cout << "};\n";
}

int main()
{
    std::vector<int> v {5, 10, 6, 4, 3, 2, 6, 7, 9, 3};
    printVec(v);

    auto m = v.begin() + v.size() / 2;
    std::nth_element(v.begin(), m, v.end());
    std::cout << "\nThe median is " << v[v.size() / 2] << '\n';
    // The consequence of the inequality of elements before/after the Nth one:
    assert(std::accumulate(v.begin(), m, 0) < std::accumulate(m, v.end(), 0));
    printVec(v);

    // Note: comp function changed
    std::nth_element(v.begin(), v.begin() + 1, v.end(), std::greater{});
    std::cout << "\nThe second largest element is " << v[1] << '\n';
    std::cout << "The largest element is " << v[0] << '\n';
    printVec(v);
}
```

Possible output:

```text
v = {5, 10, 6, 4, 3, 2, 6, 7, 9, 3};

The median is 6
v = {3, 2, 3, 4, 5, 6, 10, 7, 9, 6};

The second largest element is 9
The largest element is 10
v = {10, 9, 6, 7, 6, 3, 5, 4, 3, 2};
```

### See also

- **max_element** — returns the largest element in a range (function template)
- **min_element** — returns the smallest element in a range (function template)
- **partial_sort_copy** — copies and partially sorts a range of elements
  (function template)
- **stable_sort** — sorts a range of elements while preserving order between
  equal elements (function template)
- **sort** — sorts a range into ascending order (function template)
- **ranges::nth_element (C++20)** — partially sorts the given range making sure
  that it is partitioned by the given element (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/nth_element*
