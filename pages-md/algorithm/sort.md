# std::sort

```cpp
template< class RandomIt >
void sort( RandomIt first, RandomIt last );  // (until C++20)
template< class RandomIt >
constexpr void sort( RandomIt first, RandomIt last );  // (since C++20)
template< class ExecutionPolicy, class RandomIt >
void sort( ExecutionPolicy&& policy,
           RandomIt first, RandomIt last );  // (2) (since C++17)
template< class RandomIt, class Compare >
void sort( RandomIt first, RandomIt last, Compare comp );  // (until C++20)
template< class RandomIt, class Compare >
constexpr void sort( RandomIt first, RandomIt last, Compare comp );  // (since C++20)
template< class ExecutionPolicy, class RandomIt, class Compare >
void sort( ExecutionPolicy&& policy,
           RandomIt first, RandomIt last, Compare comp );  // (4) (since C++17)
```

Sorts the elements in the range `[``first``,``last``)` in non-descending order.
The order of equal elements is not guaranteed to be preserved.

A sequence is sorted with respect to a comparator `comp` if for any iterator
`it` pointing to the sequence and any non-negative integer `n` such that `it +
n` is a valid iterator pointing to an element of the sequence, `comp(*(it + n),
*it)` (or `*(it + n) < *it`) evaluates to `false`.

1) Elements are compared using `operator<`.

3) Elements are compared using the given binary comparison function `comp`.

2,4) Same as (1,3), but executed according to `policy`. These overloads do not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

### Parameters

- **first, last** — the range of elements to sort
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

**-`Compare` must meet the requirements of Compare.**

### Return value

(none)

### Complexity

O(N·log(N)) comparisons, where `N` is `std::distance(first, last)`.

### Exceptions

The overloads with a template parameter named `ExecutionPolicy` report errors as
follows:

- If execution of a function invoked as part of the algorithm throws an
  exception and `ExecutionPolicy` is one of the standard policies,
  `std::terminate` is called. For any other `ExecutionPolicy`, the behavior is
  implementation-defined.
- If the algorithm fails to allocate memory, `std::bad_alloc` is thrown.

### Notes

Before LWG713, the complexity requirement allowed `sort()` to be implemented
using only Quicksort, which may need O(N2) comparisons in the worst case.

Introsort can handle all cases with O(N·log(N)) comparisons (without incurring
additional overhead in the average case), and thus is usually used for
implementing `sort()`.

libc++ has not implemented the corrected time complexity requirement until LLVM
14.

### Possible implementation

See also the implementations in libstdc++ and libc++.

### Example

```cpp
#include <algorithm>
#include <array>
#include <functional>
#include <iostream>
#include <string_view>

int main()
{
    std::array<int, 10> s {5, 7, 4, 2, 8, 6, 1, 9, 0, 3};

    auto print = [&s](std::string_view const rem)
    {
        for (auto a : s)
            std::cout << a << ' ';
        std::cout << ": " << rem << '\n';
    };

    std::sort(s.begin(), s.end());
    print("sorted with the default operator<");

    std::sort(s.begin(), s.end(), std::greater<int>());
    print("sorted with the standard library compare function object");

    struct
    {
        bool operator()(int a, int b) const { return a < b; }
    }
    customLess;

    std::sort(s.begin(), s.end(), customLess);
    print("sorted with a custom function object");

    std::sort(s.begin(), s.end(), [](int a, int b)
                                  {
                                      return a > b;
                                  });
    print("sorted with a lambda expression");
}
```

Output:

```text
0 1 2 3 4 5 6 7 8 9 : sorted with the default operator<
9 8 7 6 5 4 3 2 1 0 : sorted with the standard library compare function object
0 1 2 3 4 5 6 7 8 9 : sorted with a custom function object
9 8 7 6 5 4 3 2 1 0 : sorted with a lambda expression
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 713 | C++98 | the O(N·log(N)) time complexity was only required on the
      average | it is required for the worst case

### See also

- **partial_sort** — sorts the first N elements of a range (function template)
- **stable_sort** — sorts a range of elements while preserving order between
  equal elements (function template)
- **ranges::sort (C++20)** — sorts a range into ascending order (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/sort*
