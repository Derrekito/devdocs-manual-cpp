# std::is_permutation

```cpp
template< class ForwardIt1, class ForwardIt2 >
bool is_permutation( ForwardIt1 first1, ForwardIt1 last1,
                     ForwardIt2 first2 );  // (since C++11) (until C++20)
template< class ForwardIt1, class ForwardIt2 >
constexpr bool is_permutation( ForwardIt1 first1, ForwardIt1 last1,
                               ForwardIt2 first2 );  // (since C++20)
template< class ForwardIt1, class ForwardIt2, class BinaryPredicate >
bool is_permutation( ForwardIt1 first1, ForwardIt1 last1,
                     ForwardIt2 first2, BinaryPredicate p );  // (since C++11) (until C++20)
template< class ForwardIt1, class ForwardIt2, class BinaryPredicate >
constexpr bool is_permutation( ForwardIt1 first1, ForwardIt1 last1,
                               ForwardIt2 first2, BinaryPredicate p );  // (since C++20)
template< class ForwardIt1, class ForwardIt2 >
bool is_permutation( ForwardIt1 first1, ForwardIt1 last1,
                     ForwardIt2 first2, ForwardIt2 last2 );  // (since C++14) (until C++20)
template< class ForwardIt1, class ForwardIt2 >
constexpr bool is_permutation( ForwardIt1 first1, ForwardIt1 last1,
                               ForwardIt2 first2, ForwardIt2 last2 );  // (since C++20)
template< class ForwardIt1, class ForwardIt2, class BinaryPredicate >
bool is_permutation( ForwardIt1 first1, ForwardIt1 last1,
                     ForwardIt2 first2, ForwardIt2 last2,
                     BinaryPredicate p );  // (since C++14) (until C++20)
template< class ForwardIt1, class ForwardIt2, class BinaryPredicate >
constexpr bool is_permutation( ForwardIt1 first1, ForwardIt1 last1,
                               ForwardIt2 first2, ForwardIt2 last2,
                               BinaryPredicate p );  // (since C++20)
```

Returns `true` if there exists a permutation of the elements in the range
`[``first1``,``last1``)` that makes that range equal to the range
`[``first2``,``last2``)`, where `last2` denotes `first2 + (last1 - first1)` if
it was not given.

1,3) Elements are compared using `operator==`. The behavior is undefined if it
   is not an equivalence relation.

2,4) Elements are compared using the given binary predicate `p`. The behavior is
   undefined if it is not an equivalence relation.

### Parameters

- **first1, last1** — the range of elements to compare
- **first2, last2** — the second range to compare
- **p** — binary predicate which returns `true` if the elements should be
  treated as equal. The signature of the predicate function should be equivalent
  to the following: `bool pred(const Type &a, const Type &b);` `Type` should be
  the value type of both `ForwardIt1` and `ForwardIt2`. The signature does not
  need to have `const &`, but the function must not modify the objects passed to
  it.

**Type requirements**

**-`ForwardIt1, ForwardIt2` must meet the requirements of LegacyForwardIterator.**

**-`ForwardIt1, ForwardIt2` must have the same value type.**

### Return value

`true` if the range `[``first1``,``last1``)` is a permutation of the range
`[``first2``,``last2``)`.

### Complexity

At most \(\scriptsize \mathcal{O}(N^2)\)O(N2) applications of the predicate, or
exactly \(\scriptsize N\)N if the sequences are already equal, where
\(\scriptsize N\)N is `std::distance(first1, last1)`.

However if `ForwardIt1` and `ForwardIt2` meet the requirements of
LegacyRandomAccessIterator and `std::distance(first1, last1) !=
std::distance(first2, last2)` no applications of the predicate are made.

### Note

The `std::is_permutation` can be used in *testing*, namely to check the
correctness of rearranging algorithms (e.g. sorting, shuffling, partitioning).
If `x` is an original range and `y` is a *permuted* range then
`std::is_permutation(x, y) == true` means that `y` consist of *"the same"*
elements, maybe staying at other positions.

### Possible implementation

```cpp
template<class ForwardIt1, class ForwardIt2>
bool is_permutation(ForwardIt1 first, ForwardIt1 last,
                    ForwardIt2 d_first)
{
    // skip common prefix
    std::tie(first, d_first) = std::mismatch(first, last, d_first);
    // iterate over the rest, counting how many times each element
    // from [first, last) appears in [d_first, d_last)
    if (first != last)
    {
        ForwardIt2 d_last = std::next(d_first, std::distance(first, last));
        for (ForwardIt1 i = first; i != last; ++i)
        {
            if (i != std::find(first, i, *i))
                continue; // this *i has been checked

            auto m = std::count(d_first, d_last, *i);
            if (m == 0 || std::count(i, last, *i) != m)
                return false;
        }
    }
    return true;
}
```

### Example

```cpp
#include <algorithm>
#include <iostream>

template<typename Os, typename V>
Os& operator<<(Os& os, V const& v)
{
    os << "{ ";
    for (auto const& e : v)
        os << e << ' ';
    return os << '}';
}

int main()
{
    static constexpr auto v1 = {1, 2, 3, 4, 5};
    static constexpr auto v2 = {3, 5, 4, 1, 2};
    static constexpr auto v3 = {3, 5, 4, 1, 1};

    std::cout << v2 << " is a permutation of " << v1 << ": " << std::boolalpha
              << std::is_permutation(v1.begin(), v1.end(), v2.begin()) << '\n'
              << v3 << " is a permutation of " << v1 << ": "
              << std::is_permutation(v1.begin(), v1.end(), v3.begin()) << '\n';
}
```

Output:

```text
{ 3 5 4 1 2 } is a permutation of { 1 2 3 4 5 }: true
{ 3 5 4 1 1 } is a permutation of { 1 2 3 4 5 }: false
```

### See also

- **next_permutation** — generates the next greater lexicographic permutation of
  a range of elements (function template)
- **prev_permutation** — generates the next smaller lexicographic permutation of
  a range of elements (function template)
- **equivalence_relation (C++20)** — specifies that a `relation` imposes an
  equivalence relation (concept)
- **ranges::is_permutation (C++20)** — determines if a sequence is a permutation
  of another sequence (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/is_permutation*
