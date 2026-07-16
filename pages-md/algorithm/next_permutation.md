# std::next_permutation

```cpp
template< class BidirIt >
          bool next_permutation( BidirIt first, BidirIt last );  // (until C++20)
template< class BidirIt >
constexpr bool next_permutation( BidirIt first, BidirIt last );  // (since C++20)
template< class BidirIt, class Compare >
          bool next_permutation( BidirIt first, BidirIt last, Compare comp );  // (until C++20)
template< class BidirIt, class Compare >
constexpr bool next_permutation( BidirIt first, BidirIt last, Compare comp );  // (since C++20)
```

Permutes the range `[``first``,``last``)` into the next permutation, where the
set of all permutations is ordered lexicographically with respect to `operator<`
or `comp`. Returns `true` if such a "next permutation" exists; otherwise
transforms the range into the lexicographically first permutation (as if by
`std::sort(first, last, comp)`) and returns `false`.

### Parameters

- **first, last** — the range of elements to permute
- **comp** — comparison function object (i.e. an object that satisfies the
  requirements of Compare) which returns `true` if the first argument is *less*
  than the second. The signature of the comparison function should be equivalent
  to the following: `bool cmp(const Type1& a, const Type2& b);` While the
  signature does not need to have `const&`, the function must not modify the
  objects passed to it and must be able to accept all values of type (possibly
  const) `Type1` and `Type2` regardless of value category (thus, `Type1&` is not
  allowed, nor is `Type1` unless for `Type1` a move is equivalent to a
  copy(since C++11)). The types Type1 and Type2 must be such that an object of
  type BidirIt can be dereferenced and then implicitly converted to both of
  them.

**Type requirements**

**-`BidirIt` must meet the requirements of ValueSwappable and LegacyBidirectionalIterator.**

### Return value

`true` if the new permutation is lexicographically greater than the old. `false`
if the last permutation was reached and the range was reset to the first
permutation.

### Exceptions

Any exceptions thrown from iterator operations or the element swap.

### Complexity

At most N/2 swaps, where `N = std::distance(first, last)`. Averaged over the
entire sequence of permutations, typical implementations use about 3 comparisons
and 1.5 swaps per call.

### Notes

Implementations (e.g. MSVC STL) may enable vectorization when the iterator type
satisfies LegacyContiguousIterator and swapping its value type calls neither
non-trivial special member function nor ADL-found `swap`.

### Possible implementation

```cpp
template<class BidirIt>
bool next_permutation(BidirIt first, BidirIt last)
{
    auto r_first = std::make_reverse_iterator(last);
    auto r_last = std::make_reverse_iterator(first);
    auto left = std::is_sorted_until(r_first, r_last);

    if (left != r_last)
    {
        auto right = std::upper_bound(r_first, left, *left);
        std::iter_swap(left, right);
    }

    std::reverse(left.base(), last);
    return left != r_last;
}
```

### Example

The following code prints all three permutations of the string "aba".

```cpp
#include <algorithm>
#include <iostream>
#include <string>

int main()
{
    std::string s = "aba";

    do std::cout << s << '\n';
    while (std::next_permutation(s.begin(), s.end()));

    std::cout << s << '\n';
}
```

Output:

```text
aba
baa
aab
```

### See also

- **is_permutation (C++11)** — determines if a sequence is a permutation of
  another sequence (function template)
- **prev_permutation** — generates the next smaller lexicographic permutation of
  a range of elements (function template)
- **ranges::next_permutation (C++20)** — generates the next greater
  lexicographic permutation of a range of elements (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/next_permutation*
