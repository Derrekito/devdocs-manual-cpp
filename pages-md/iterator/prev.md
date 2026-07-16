# std::prev

```cpp
template< class BidirIt >
BidirIt prev( BidirIt it, typename std::iterator_traits<BidirIt>::difference_type n = 1 );  // (since C++11) (until C++17)
template< class BidirIt >
constexpr
BidirIt prev( BidirIt it, typename std::iterator_traits<BidirIt>::difference_type n = 1 );  // (since C++17)
```

Return the `n`th predecessor (or `-n`th successor if `n` is negative) of
iterator `it`.

### Parameters

- **it** — an iterator
- **n** — number of elements `it` should be descended

**Type requirements**

**-`BidirIt` must meet the requirements of LegacyBidirectionalIterator.**

### Return value

An iterator of type `BidirIt` that holds the `n`th predecessor (or `-n`th
successor if `n` is negative) of iterator `it`.

### Complexity

Linear.

However, if `BidirIt` additionally meets the requirements of
LegacyRandomAccessIterator, complexity is constant.

### Possible implementation

```cpp
template<class BidirIt>
constexpr // since C++17
BidirIt prev(BidirIt it, typename std::iterator_traits<BidirIt>::difference_type n = 1)
{
    std::advance(it, -n);
    return it;
}
```

### Notes

Although the expression `--c.end()` often compiles, it is not guaranteed to do
so: `c.end()` is an rvalue expression, and there is no iterator requirement that
specifies that decrement of an rvalue is guaranteed to work. In particular, when
iterators are implemented as pointers or its `operator--` is
lvalue-ref-qualified, `--c.end()` does not compile, while `std::prev(c.end())`
does.

### Example

```cpp
#include <iostream>
#include <iterator>
#include <vector>

int main()
{
    std::vector<int> v{3, 1, 4};

    auto it = v.end();
    auto pv = std::prev(it, 2);
    std::cout << *pv << '\n';

    it = v.begin();
    pv = std::prev(it, -2);
    std::cout << *pv << '\n';
}
```

Output:

```text
1
4
```

### See also

- **next (C++11)** — increment an iterator (function template)
- **advance** — advances an iterator by given distance (function template)
- **distance** — returns the distance between two iterators (function template)
- **ranges::prev (C++20)** — decrement an iterator by a given distance or to a
  bound (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/iterator/prev*
