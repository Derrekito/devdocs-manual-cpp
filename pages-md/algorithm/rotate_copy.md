# std::rotate_copy

```cpp
template< class ForwardIt, class OutputIt >
OutputIt rotate_copy( ForwardIt first, ForwardIt n_first,
                      ForwardIt last, OutputIt d_first );  // (until C++20)
template< class ForwardIt, class OutputIt >
constexpr OutputIt rotate_copy( ForwardIt first, ForwardIt n_first,
                                ForwardIt last, OutputIt d_first );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2 >
ForwardIt2 rotate_copy( ExecutionPolicy&& policy,
                        ForwardIt1 first, ForwardIt1 n_first,
                        ForwardIt1 last, ForwardIt2 d_first );  // (2) (since C++17)
```

1) Copies the elements from the range `[``first``,``last``)`, to another range
   beginning at `d_first` in such a way, that the element `*(n_first)` becomes
   the first element of the new range and `*(n_first - 1)` becomes the last
   element.

The behavior is undefined if either `[``first``,``n_first``)` or
   `[``n_first``,``last``)` is not a valid range, or the source and destination
   ranges overlap.

2) Same as (1), but executed according to `policy`. This overload does not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

### Parameters

- **first, last** — the range of elements to copy
- **n_first** — an iterator to an element in `[``first``,``last``)` that should
  appear at the beginning of the new range
- **d_first** — beginning of the destination range
- **policy** — the execution policy to use. See execution policy for details.

**Type requirements**

**-`ForwardIt, ForwardIt1, ForwardIt2` must meet the requirements of LegacyForwardIterator.**

**-`OutputIt` must meet the requirements of LegacyOutputIterator.**

### Return value

Output iterator to the element past the last element copied.

### Complexity

linear in the distance between `first` and `last`.

### Exceptions

The overload with a template parameter named `ExecutionPolicy` reports errors as
follows:

- If execution of a function invoked as part of the algorithm throws an
  exception and `ExecutionPolicy` is one of the standard policies,
  `std::terminate` is called. For any other `ExecutionPolicy`, the behavior is
  implementation-defined.
- If the algorithm fails to allocate memory, `std::bad_alloc` is thrown.

### Possible implementation

See also the implementations in libstdc++, libc++, and MSVC STL.

```cpp
template<class ForwardIt, class OutputIt>
constexpr // since C++20
OutputIt rotate_copy(ForwardIt first, ForwardIt n_first,
                     ForwardIt last, OutputIt d_first)
{
    d_first = std::copy(n_first, last, d_first);
    return std::copy(first, n_first, d_first);
}
```

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <vector>

int main()
{
    std::vector<int> src {1, 2, 3, 4, 5};
    std::vector<int> dest(src.size());
    auto pivot = std::find(src.begin(), src.end(), 3);

    std::rotate_copy(src.begin(), pivot, src.end(), dest.begin());
    for (int i : dest)
        std::cout << i << ' ';
    std::cout << '\n';

    // copy the rotation result directly to the std::cout
    pivot = std::find(dest.begin(), dest.end(), 1);
    std::rotate_copy(dest.begin(), pivot, dest.end(),
                     std::ostream_iterator<int>(std::cout, " "));
    std::cout << '\n';
}
```

Output:

```text
3 4 5 1 2
1 2 3 4 5
```

### See also

- **rotate** — rotates the order of elements in a range (function template)
- **ranges::rotate_copy (C++20)** — copies and rotate a range of elements
  (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/rotate_copy*
