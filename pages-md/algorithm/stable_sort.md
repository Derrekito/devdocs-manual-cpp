# std::stable_sort

```cpp
template< class RandomIt >
void stable_sort( RandomIt first, RandomIt last );  // (until C++26)
template< class RandomIt >
constexpr void stable_sort( RandomIt first, RandomIt last );  // (since C++26)
template< class ExecutionPolicy, class RandomIt >
void stable_sort( ExecutionPolicy&& policy,
                  RandomIt first, RandomIt last );  // (2) (since C++17)
template< class RandomIt, class Compare >
void stable_sort( RandomIt first, RandomIt last, Compare comp );  // (until C++26)
template< class RandomIt, class Compare >
constexpr void stable_sort( RandomIt first, RandomIt last, Compare comp );  // (since C++26)
template< class ExecutionPolicy, class RandomIt, class Compare >
void stable_sort( ExecutionPolicy&& policy,
                  RandomIt first, RandomIt last, Compare comp );  // (4) (since C++17)
```

Sorts the elements in the range `[``first``,``last``)` in non-descending order.
The order of equivalent elements is guaranteed to be preserved.

A sequence is sorted with respect to a comparator `comp` if for any iterator
`it` pointing to the sequence and any non-negative integer `n` such that `it +
n` is a valid iterator pointing to an element of the sequence, `comp(*(it + n),
*it)` (or `*(it + n) < *it`) evaluates to `false`.

1) Elements are compared using `operator<`.

3) Elements are compared using the given comparison function `comp`.

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

### Return value

(none)

### Complexity

O(N·log(N)2), where `N = std::distance(first, last)` applications of `cmp`. If
additional memory is available, then the complexity is O(N·log(N)).

### Exceptions

The overloads with a template parameter named `ExecutionPolicy` report errors as
follows:

- If execution of a function invoked as part of the algorithm throws an
  exception and `ExecutionPolicy` is one of the standard policies,
  `std::terminate` is called. For any other `ExecutionPolicy`, the behavior is
  implementation-defined.
- If the algorithm fails to allocate memory, `std::bad_alloc` is thrown.

### Notes

This function attempts to allocate a temporary buffer equal in size to the
sequence to be sorted. If the allocation fails, the less efficient algorithm is
chosen.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_constexpr_algorithms` | 202306L | `constexpr` stable sorting

### Possible implementation

See also the implementations in libstdc++ and libc++.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <string>
#include <vector>

struct Employee
{
    int age;
    std::string name; // Does not participate in comparisons
};

bool operator<(const Employee& lhs, const Employee& rhs)
{
    return lhs.age < rhs.age;
}

int main()
{
    std::vector<Employee> v{{108, "Zaphod"}, {32, "Arthur"}, {108, "Ford"}};

    std::stable_sort(v.begin(), v.end());

    for (const Employee& e : v)
        std::cout << e.age << ", " << e.name << '\n';
}
```

Output:

```text
32, Arthur
108, Zaphod
108, Ford
```

### See also

- **sort** — sorts a range into ascending order (function template)
- **partial_sort** — sorts the first N elements of a range (function template)
- **stable_partition** — divides elements into two groups while preserving their
  relative order (function template)
- **ranges::stable_sort (C++20)** — sorts a range of elements while preserving
  order between equal elements (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/stable_sort*
