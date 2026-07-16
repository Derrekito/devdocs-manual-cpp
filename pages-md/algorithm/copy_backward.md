# std::copy_backward

```cpp
template< class BidirIt1, class BidirIt2 >
BidirIt2 copy_backward( BidirIt1 first, BidirIt1 last, BidirIt2 d_last );  // (until C++20)
template< class BidirIt1, class BidirIt2 >
constexpr BidirIt2 copy_backward( BidirIt1 first, BidirIt1 last, BidirIt2 d_last );  // (since C++20)
```

Copies the elements from the range `[``first``,``last``)` to another range
ending at `d_last`. The elements are copied in reverse order (the last element
is copied first), but their relative order is preserved.

The behavior is undefined if `d_last` is within `(``first``,``last``)`.
`std::copy` must be used instead of `std::copy_backward` in that case.

### Parameters

- **first, last** — the range of the elements to copy from
- **d_last** — the end of the destination range

**Type requirements**

**-`BidirIt` must meet the requirements of LegacyBidirectionalIterator.**

### Return value

Iterator to the last element copied.

### Complexity

Exactly `last - first` assignments.

### Notes

When copying overlapping ranges, `std::copy` is appropriate when copying to the
left (beginning of the destination range is outside the source range) while
`std::copy_backward` is appropriate when copying to the right (end of the
destination range is outside the source range).

### Possible implementation

```cpp
template<class BidirIt1, class BidirIt2>
BidirIt2 copy_backward(BidirIt1 first, BidirIt1 last, BidirIt2 d_last)
{
    while (first != last)
        *(--d_last) = *(--last);
    return d_last;
}
```

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <numeric>
#include <vector>

int main()
{
    std::vector<int> source(4);
    std::iota(source.begin(), source.end(), 1); // fills with 1, 2, 3, 4

    std::vector<int> destination(6);

    std::copy_backward(source.begin(), source.end(), destination.end());

    std::cout << "destination contains: ";
    for (auto i: destination)
        std::cout << i << ' ';
    std::cout << '\n';
}
```

Output:

```text
destination contains: 0 0 1 2 3 4
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 1206 | C++98 | 1. the behavior was well-defined if `d_last == last` 2. the
      behavior was undefined if `d_last == first` | 1. made undefined 2. made
      well-defined

### See also

- **copycopy_if (C++11)** — copies a range of elements to a new location
  (function template)
- **ranges::copy_backward (C++20)** — copies a range of elements in backwards
  order (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/copy_backward*
