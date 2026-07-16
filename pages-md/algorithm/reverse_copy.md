# std::reverse_copy

```cpp
template< class BidirIt, class OutputIt >
OutputIt reverse_copy( BidirIt first, BidirIt last, OutputIt d_first );  // (until C++20)
template< class BidirIt, class OutputIt >
constexpr OutputIt reverse_copy( BidirIt first, BidirIt last,
                                 OutputIt d_first );  // (since C++20)
template< class ExecutionPolicy, class BidirIt, class ForwardIt >
ForwardIt reverse_copy( ExecutionPolicy&& policy,
                        BidirIt first, BidirIt last, ForwardIt d_first );  // (2) (since C++17)
```

1) Given \(\scriptsize N\)N as `std::distance(first, last)`. Copies the elements
   from the range `[``first``,``last``)` to another range of \(\scriptsize N\)N
   elements beginning at `d_first` (destination range) in such a way that the
   elements in the destination range are in reverse order.

Behaves as if by executing the assignment `*(d_first + N - 1 - i) = *(first +
   i)`[1] once for each integer `i` in `[``​0​``,``N``)`.

If `[``first``,``last``)` and the destination range overlap, the behavior is
   undefined.

2) Same as (1), but executed according to `policy`. This overload does not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

### Parameters

- **first, last** — the range of elements to copy
- **d_first** — the beginning of the destination range

**Type requirements**

**-`BidirIt` must meet the requirements of LegacyBidirectionalIterator.**

**-`OutputIt` must meet the requirements of LegacyOutputIterator.**

**-`ForwardIt` must meet the requirements of LegacyForwardIterator.**

### Return value

Output iterator to the element past the last element copied.

### Complexity

Exactly \(\scriptsize N\)N assignments.

### Exceptions

The overload with a template parameter named `ExecutionPolicy` reports errors as
follows:

- If execution of a function invoked as part of the algorithm throws an
  exception and `ExecutionPolicy` is one of the standard policies,
  `std::terminate` is called. For any other `ExecutionPolicy`, the behavior is
  implementation-defined.
- If the algorithm fails to allocate memory, `std::bad_alloc` is thrown.

### Notes

Implementations (e.g. MSVC STL) may enable vectorization when the both iterator
types satisfy LegacyContiguousIterator and have the same value type, and the
value type is TriviallyCopyable.

### Possible implementation

See also the implementations in libstdc++, libc++, and MSVC STL.

```cpp
template<class BidirIt, class OutputIt>
constexpr // since C++20
OutputIt reverse_copy(BidirIt first, BidirIt last, OutputIt d_first)
{
    for (; first != last; ++d_first)
        *d_first = *(--last);
    return d_first;
}
```

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main()
{
    auto print = [](const std::vector<int>& v)
    {
        for (const auto& value : v)
            std::cout << value << ' ';
        std::cout << '\n';
    };

    std::vector<int> v{1, 2, 3};
    print(v);

    std::vector<int> destination(3);
    std::reverse_copy(std::begin(v), std::end(v), std::begin(destination));
    print(destination);

    std::reverse_copy(std::rbegin(v), std::rend(v), std::begin(destination));
    print(destination);
}
```

Output:

```text
1 2 3
3 2 1
1 2 3
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2074 | C++98 | for each `i`, the assignment was `*(d_first + N - i) =
      *(first + i)`[1] | corrected to `*(d_first + N - 1 - i) = *(first + i)`[1]

1. LegacyOutputIterator is not required to support binary `+` and `-`. The
   usages of `+` and `-` here are exposition-only: the actual computation does
   not need to use them.

### See also

- **reverse** — reverses the order of elements in a range (function template)
- **ranges::reverse_copy (C++20)** — creates a copy of a range that is reversed
  (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/reverse_copy*
