# std::binary_search

```cpp
template< class ForwardIt, class T >
bool binary_search( ForwardIt first, ForwardIt last, const T& value );  // (until C++20)
template< class ForwardIt, class T >
constexpr bool binary_search( ForwardIt first, ForwardIt last,
                              const T& value );  // (since C++20)
template< class ForwardIt, class T, class Compare >
bool binary_search( ForwardIt first, ForwardIt last,
                    const T& value, Compare comp );  // (until C++20)
template< class ForwardIt, class T, class Compare >
constexpr bool binary_search( ForwardIt first, ForwardIt last,
                              const T& value, Compare comp );  // (since C++20)
```

Checks if an element equivalent to `value` appears within the range
`[``first``,``last``)`.

For `std::binary_search` to succeed, the range `[``first``,``last``)` must be at
least partially ordered with respect to `value`, i.e. it must satisfy all of the
following requirements:

- partitioned with respect to `element < value` or `comp(element, value)` (that
  is, all elements for which the expression is `true` precede all elements for
  which the expression is `false`).
- partitioned with respect to `!(value < element)` or `!comp(value, element)`.
- for all elements, if `element < value` or `comp(element, value)` is `true`
  then `!(value < element)` or `!comp(value, element)` is also `true`.

A fully-sorted range meets these criteria.

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

`true` if an element equal to `value` is found, `false` otherwise.

### Complexity

The number of comparisons performed is logarithmic in the distance between
`first` and `last` (at most log2(last - first) + O(1) comparisons). However, for
non-LegacyRandomAccessIterators, number of iterator increments is linear.

### Possible implementation

See also the implementations in libstdc++ and libc++.

```cpp
template<class ForwardIt, class T>
bool binary_search(ForwardIt first, ForwardIt last, const T& value)
{
    first = std::lower_bound(first, last, value);
    return (!(first == last) and !(value < *first));
}
```

```cpp
template<class ForwardIt, class T, class Compare>
bool binary_search(ForwardIt first, ForwardIt last, const T& value, Compare comp)
{
    first = std::lower_bound(first, last, value, comp);
    return (!(first == last) and !(comp(value, *first)));
}
```

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> haystack{1, 3, 4, 5, 9};
    std::vector<int> needles{1, 2, 3};

    for (const auto needle : needles)
    {
        std::cout << "Searching for " << needle << '\n';
        if (std::binary_search(haystack.begin(), haystack.end(), needle))
            std::cout << "Found " << needle << '\n';
        else
            std::cout << "No dice!\n";
    }
}
```

Output:

```text
Searching for 1
Found 1
Searching for 2
no dice!
Searching for 3
Found 3
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 270 | C++98 | `Compare` was required to satisfy Compare and `T` was
      required to be LessThanComparable (strict weak ordering required) | only a
      partitioning is required; heterogeneous comparisons permitted
  LWG 787 | C++98 | at most log(last - first) + 2 comparisons were allowed |
      corrected to log2(last - first) + O(1)

### See also

- **equal_range** — returns range of elements matching a specific key (function
  template)
- **lower_bound** — returns an iterator to the first element *not less* than the
  given value (function template)
- **upper_bound** — returns an iterator to the first element *greater* than a
  certain value (function template)
- **ranges::binary_search (C++20)** — determines if an element exists in a
  partially-ordered range (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/binary_search*
