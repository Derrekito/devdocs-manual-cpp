# std::lower_bound

```cpp
template< class ForwardIt, class T >
ForwardIt lower_bound( ForwardIt first, ForwardIt last, const T& value );  // (until C++20)
template< class ForwardIt, class T >
constexpr ForwardIt lower_bound( ForwardIt first, ForwardIt last,
                                 const T& value );  // (since C++20)
template< class ForwardIt, class T, class Compare >
ForwardIt lower_bound( ForwardIt first, ForwardIt last,
                       const T& value, Compare comp );  // (until C++20)
template< class ForwardIt, class T, class Compare >
constexpr ForwardIt lower_bound( ForwardIt first, ForwardIt last,
                                 const T& value, Compare comp );  // (since C++20)
```

Returns an iterator pointing to the first element in the range
`[``first``,``last``)` such that `element < value` (or `comp(element, value)`)
is `false`, (i.e. that is greater than or equal to `value`), or `last` if no
such element is found.

The range `[``first``,``last``)` must be partitioned with respect to the
expression `element < value` (or `comp(element, value)`), i.e., all elements for
which the expression is `true` must precede all elements for which the
expression is `false`. A fully-sorted range meets this criterion.

The first version uses operator< to compare the elements, the second version
uses the given comparison function `comp`.

### Parameters

- **first, last** — iterators defining the partially-ordered range to examine
- **value** — value to compare the elements to
- **comp** — binary predicate which returns ​`true` if the first argument is
  *less* than (i.e. is ordered before) the second. The signature of the
  predicate function should be equivalent to the following: `bool pred(const
  Type1 &a, const Type2 &b);` While the signature does not need to have `const
  &`, the function must not modify the objects passed to it and must be able to
  accept all values of type (possibly const) `Type1` and `Type2` regardless of
  value category (thus, `Type1 &` is not allowed, nor is `Type1` unless for
  `Type1` a move is equivalent to a copy(since C++11)). The type Type1 must be
  such that an object of type ForwardIt can be dereferenced and then implicitly
  converted to Type1. The type Type2 must be such that an object of type T can
  be implicitly converted to Type2. ​

**Type requirements**

**-`ForwardIt` must meet the requirements of LegacyForwardIterator.**

**-`Compare` must meet the requirements of BinaryPredicate. It is not required to satisfy Compare.**

### Return value

Iterator pointing to the first element in the range `[``first``,``last``)` such
that `element < value` (or `comp(element, value)`) is false, or `last` if no
such element is found.

### Complexity

The number of comparisons performed is logarithmic in the distance between
`first` and `last` (at most log2(last - first) + O(1) comparisons). However, for
non-LegacyRandomAccessIterators, the number of iterator increments is linear.
Notably, `std::map`, `std::multimap`, `std::set`, and `std::multiset` iterators
are not random access, and so their member `lower_bound` functions should be
preferred.

### Possible implementation

See also the implementations in libstdc++ and libc++.

```cpp
template<class ForwardIt, class T>
ForwardIt lower_bound(ForwardIt first, ForwardIt last, const T& value)
{
    ForwardIt it;
    typename std::iterator_traits<ForwardIt>::difference_type count, step;
    count = std::distance(first, last);

    while (count > 0)
    {
        it = first;
        step = count / 2;
        std::advance(it, step);

        if (*it < value)
        {
            first = ++it;
            count -= step + 1;
        }
        else
            count = step;
    }

    return first;
}
```

```cpp
template<class ForwardIt, class T, class Compare>
ForwardIt lower_bound(ForwardIt first, ForwardIt last, const T& value, Compare comp)
{
    ForwardIt it;
    typename std::iterator_traits<ForwardIt>::difference_type count, step;
    count = std::distance(first, last);

    while (count > 0)
    {
        it = first;
        step = count / 2;
        std::advance(it, step);

        if (comp(*it, value))
        {
            first = ++it;
            count -= step + 1;
        }
        else
            count = step;
    }

    return first;
}
```

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

struct PriceInfo { double price; };

int main()
{
    const std::vector<int> data{1, 2, 4, 5, 5, 6};

    for (int i = 0; i < 8; ++i)
    {
        // Search for first element x such that i ≤ x
        auto lower = std::lower_bound(data.begin(), data.end(), i);

        std::cout << i << " ≤ ";
        lower != data.end()
            ? std::cout << *lower << " at index " << std::distance(data.begin(), lower)
            : std::cout << "not found";
        std::cout << '\n';
    }

    std::vector<PriceInfo> prices{{100.0}, {101.5}, {102.5}, {102.5}, {107.3}};

    for (const double to_find : {102.5, 110.2})
    {
        auto prc_info = std::lower_bound(prices.begin(), prices.end(), to_find,
            [](const PriceInfo& info, double value)
            {
                return info.price < value;
            });

        prc_info != prices.end()
            ? std::cout << prc_info->price << " at index " << prc_info - prices.begin()
            : std::cout << to_find << " not found";
        std::cout << '\n';
    }
}
```

Output:

```text
0 ≤ 1 at index 0
1 ≤ 1 at index 0
2 ≤ 2 at index 1
3 ≤ 4 at index 2
4 ≤ 4 at index 2
5 ≤ 5 at index 3
6 ≤ 6 at index 5
7 ≤ not found
102.5 at index 2
110.2 not found
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 270 | C++98 | `Compare` was required to satisfy Compare and `T` was
      required to be LessThanComparable (strict weak ordering required) | only a
      partitioning is required; heterogeneous comparisons permitted
  LWG 384 | C++98 | at most log(last - first) + 1 comparisons were allowed |
      corrected to log2(last - first) + O(1)

### See also

- **equal_range** — returns range of elements matching a specific key (function
  template)
- **partition** — divides a range of elements into two groups (function
  template)
- **partition_point (C++11)** — locates the partition point of a partitioned
  range (function template)
- **upper_bound** — returns an iterator to the first element *greater* than a
  certain value (function template)
- **lower_bound** — returns an iterator to the first element *not less* than the
  given key (public member function of `std::set<Key,Compare,Allocator>`)
- **lower_bound** — returns an iterator to the first element *not less* than the
  given key (public member function of `std::multiset<Key,Compare,Allocator>`)
- **ranges::lower_bound (C++20)** — returns an iterator to the first element
  *not less* than the given value (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/lower_bound*
