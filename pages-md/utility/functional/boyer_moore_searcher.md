# std::boyer_moore_searcher

```cpp
template< class RandomIt1,
          class Hash = std::hash<typename std::iterator_traits<RandomIt1>::value_type>,
          class BinaryPredicate = std::equal_to<> >
class boyer_moore_searcher;  // (since C++17)
```

A searcher suitable for use with the Searcher overload of `std::search` that
implements the Boyer-Moore string searching algorithm.

`std::boyer_moore_searcher` is CopyConstructible and CopyAssignable.

`RandomIt1` must meet the requirements of LegacyRandomAccessIterator.

### Member functions

## std::boyer_moore_searcher::boyer_moore_searcher

```cpp
boyer_moore_searcher( RandomIt1 pat_first,
                      RandomIt1 pat_last,
                      Hash hf = Hash(),
                      BinaryPredicate pred = BinaryPredicate() );
```

Constructs a `std::boyer_moore_searcher` by storing copies of `pat_first`,
`pat_last`, `hf`, and `pred`, setting up any necessary internal data structures.

The value type of `RandomIt1` must be DefaultConstructible, CopyConstructible
and CopyAssignable.

For any two values `A` and `B` of the type
`std::iterator_traits<RandomIt1>::value_type`, if `pred(A, B) == true`, then
`hf(A) == hf(B)` shall be `true`.

### Parameters

- **pat_first, pat_last** — a pair of iterators designating the string to be
  searched for
- **hf** — a callable object used to hash the elements of the string
- **pred** — a callable object used to determine equality

### Exceptions

Any exceptions thrown by

- the copy constructor of `RandomIt1`;
- the default constructor, copy constructor, and copy assignment operator of the
  value type of `RandomIt1`; or
- the copy constructor and function call operator of `BinaryPredicate` or
  `Hash`.

May also throw `std::bad_alloc` if additional memory required for internal data
structures cannot be allocated.

## std::boyer_moore_searcher::operator()

```cpp
template< class RandomIt2 >
std::pair<RandomIt2, RandomIt2> operator()( RandomIt2 first, RandomIt2 last ) const;  // (since C++17)
```

The member function called by the Searcher overload of `std::search` to perform
a search with this searcher. `RandomIt2` must meet the requirements of
LegacyRandomAccessIterator.

`RandomIt1` and `RandomIt2` must have the same value type.

### Parameters

- **first, last** — a pair of iterators designating the string to be examined

### Return value

If the pattern `[``pat_first``,``pat_last``)` is empty, returns
`std::make_pair(first, first)`.

Otherwise, returns a pair of iterators to the first and one past last positions
in `[``first``,``last``)` where a subsequence that compares equal to
`[``pat_first``,``pat_last``)` as defined by `pred` is located, or
`std::make_pair(last, last)` otherwise.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_boyer_moore_searcher` | 201603L | (C++17) | searchers

### Example

```cpp
#include <algorithm>
#include <functional>
#include <iomanip>
#include <iostream>
#include <string_view>

int main()
{
    constexpr std::string_view haystack =
        "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed "
        "do eiusmod tempor incididunt ut labore et dolore magna aliqua";

    const std::string_view needle{"pisci"};

    if (const auto it = std::search(haystack.begin(), haystack.end(),
            std::boyer_moore_searcher(needle.begin(), needle.end()));
        it != haystack.end()
    )
        std::cout << "The string " << std::quoted(needle) << " found at offset "
                  << it - haystack.begin() << '\n';
    else
        std::cout << "The string " << std::quoted(needle) << " not found\n";
}
```

Output:

```text
The string "pisci" found at offset 43
```

### See also

- **search** — searches for a range of elements (function template)
- **default_searcher (C++17)** — standard C++ library search algorithm
  implementation (class template)
- **boyer_moore_horspool_searcher (C++17)** — Boyer-Moore-Horspool search
  algorithm implementation (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/boyer_moore_searcher*
