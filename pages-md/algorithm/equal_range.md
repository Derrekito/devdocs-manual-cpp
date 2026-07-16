# std::equal_range

```cpp
template< class ForwardIt, class T >
std::pair<ForwardIt, ForwardIt>
    equal_range( ForwardIt first, ForwardIt last, const T& value );  // (until C++20)
template< class ForwardIt, class T >
constexpr std::pair<ForwardIt, ForwardIt>
    equal_range( ForwardIt first, ForwardIt last, const T& value );  // (since C++20)
template< class ForwardIt, class T, class Compare >
std::pair<ForwardIt, ForwardIt>
    equal_range( ForwardIt first, ForwardIt last,
                 const T& value, Compare comp );  // (until C++20)
template< class ForwardIt, class T, class Compare >
constexpr std::pair<ForwardIt, ForwardIt>
    equal_range( ForwardIt first, ForwardIt last,
                 const T& value, Compare comp );  // (since C++20)
```

Returns a range containing all elements equivalent to `value` in the range
`[``first``,``last``)`.

The range `[``first``,``last``)` must be at least partially ordered with respect
to `value`, i.e. it must satisfy all of the following requirements:

- partitioned with respect to `element < value` or `comp(element, value)` (that
  is, all elements for which the expression is `true` precedes all elements for
  which the expression is `false`).
- partitioned with respect to `!(value < element)` or `!comp(value, element)`.
- for all elements, if `element < value` or `comp(element, value)` is `true`
  then `!(value < element)` or `!comp(value, element)` is also `true`.

A fully-sorted range meets these criteria.

The returned range is defined by two iterators, one pointing to the first
element that is *not less* than `value` and another pointing to the first
element *greater* than `value`. The first iterator may be alternatively obtained
with `std::lower_bound()`, the second - with `std::upper_bound()`.

The first version uses operator< to compare the elements, the second version
uses the given comparison function `comp`.

### Parameters

- **first, last** — the range of elements to examine
- **value** — value to compare the elements to
- **comp** — binary predicate which returns ​`true` if the first argument is
  *less* than (i.e. is ordered before) the second. The signature of the
  predicate function should be equivalent to the following: `bool pred(const
  Type1 &a, const Type2 &b);` While the signature does not need to have `const
  &`, the function must not modify the objects passed to it and must be able to
  accept all values of type (possibly const) `Type1` and `Type2` regardless of
  value category (thus, `Type1 &` is not allowed, nor is `Type1` unless for
  `Type1` a move is equivalent to a copy(since C++11)). The types Type1 and
  Type2 must be such that an object of type T can be implicitly converted to
  both Type1 and Type2, and an object of type ForwardIt can be dereferenced and
  then implicitly converted to both Type1 and Type2. ​

**Type requirements**

**-`ForwardIt` must meet the requirements of LegacyForwardIterator.**

**-`Compare` must meet the requirements of BinaryPredicate. It is not required to satisfy Compare.**

### Return value

A `std::pair` containing a pair of iterators defining the wanted range. The
first pointing to the first element that is *not less* than `value` and the
second pointing to the first element *greater* than `value`.

If there are no elements *not less* than `value`, `last` is returned as the
first element. Similarly if there are no elements *greater* than `value`, `last`
is returned as the second element.

### Complexity

The number of comparisons performed is logarithmic in the distance between
`first` and `last` (At most 2 * log2(last - first) + O(1) comparisons). However,
for non-LegacyRandomAccessIterators, the number of iterator increments is
linear. Notably, `std::set` and `std::multiset` iterators are not random access,
and so their member functions `std::set::equal_range` (resp.
`std::multiset::equal_range`) should be preferred.

### Possible implementation

```cpp
template<class ForwardIt, class T>
std::pair<ForwardIt, ForwardIt>
    equal_range(ForwardIt first, ForwardIt last, const T& value)
{
    return std::make_pair(std::lower_bound(first, last, value),
                          std::upper_bound(first, last, value));
}
```

```cpp
template<class ForwardIt, class T, class Compare>
std::pair<ForwardIt, ForwardIt>
    equal_range(ForwardIt first, ForwardIt last, const T& value, Compare comp)
{
    return std::make_pair(std::lower_bound(first, last, value, comp),
                          std::upper_bound(first, last, value, comp));
}
```

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

struct S
{
    int number;
    char name;
    // note: name is ignored by this comparison operator
    bool operator<(const S& s) const { return number < s.number; }
};

struct Comp
{
    bool operator()(const S& s, int i) const { return s.number < i; }
    bool operator()(int i, const S& s) const { return i < s.number; }
};

int main()
{
    // note: not ordered, only partitioned w.r.t. S defined below
    const std::vector<S> vec{{1, 'A'}, {2, 'B'}, {2, 'C'},
                             {2, 'D'}, {4, 'G'}, {3, 'F'}};
    const S value{2, '?'};

    std::cout << "Compare using S::operator<(): ";
    const auto p = std::equal_range(vec.begin(), vec.end(), value);

    for (auto i = p.first; i != p.second; ++i)
        std::cout << i->name << ' ';

    std::cout << "\n" "Using heterogeneous comparison: ";
    const auto p2 = std::equal_range(vec.begin(), vec.end(), 2, Comp{});

    for (auto i = p2.first; i != p2.second; ++i)
        std::cout << i->name << ' ';
    std::cout << '\n';
}
```

Output:

```text
Compare using S::operator<(): B C D
Using heterogeneous comparison: B C D
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 270 | C++98 | `Compare` was required to satisfy Compare and `T` was
      required to be LessThanComparable (strict weak ordering required) | only a
      partitioning is required; heterogeneous comparisons permitted
  LWG 384 | C++98 | at most 2 * log(last - first) + 1 comparisons were allowed,
      which is not implementable[1] | corrected to 2 * log2(last - first) + O(1)

1. Applying `equal_range` to a single-element range requires 2 comparisons, but
   at most 1 comparison is allowed by the complexity requirement.

### See also

- **lower_bound** — returns an iterator to the first element *not less* than the
  given value (function template)
- **upper_bound** — returns an iterator to the first element *greater* than a
  certain value (function template)
- **binary_search** — determines if an element exists in a partially-ordered
  range (function template)
- **partition** — divides a range of elements into two groups (function
  template)
- **equal** — determines if two sets of elements are the same (function
  template)
- **equal_range** — returns range of elements matching a specific key (public
  member function of `std::set<Key,Compare,Allocator>`)
- **equal_range** — returns range of elements matching a specific key (public
  member function of `std::multiset<Key,Compare,Allocator>`)
- **ranges::equal_range (C++20)** — returns range of elements matching a
  specific key (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/equal_range*
