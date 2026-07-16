# std::default_searcher

```cpp
template< class ForwardIt, class BinaryPredicate = std::equal_to<> >
class default_searcher;  // (since C++17)
```

A class suitable for use with Searcher overload of `std::search` that delegates
the search operation to the pre-C++17 standard library's `std::search`.

`std::default_searcher` is CopyConstructible and CopyAssignable.

### Member functions

## std::default_searcher::default_searcher

```cpp
default_searcher( ForwardIt pat_first,
                  ForwardIt pat_last,
                  BinaryPredicate pred = BinaryPredicate() );  // (since C++17) (until C++20)
constexpr default_searcher( ForwardIt pat_first,
                            ForwardIt pat_last,
                            BinaryPredicate pred = BinaryPredicate() );  // (since C++20)
```

Constructs a `std::default_searcher` by storing copies of `pat_first`,
`pat_last`, and `pred`.

### Parameters

- **pat_first, pat_last** — a pair of iterators designating the string to be
  searched for
- **pred** — a callable object used to determine equality

### Exceptions

Any exceptions thrown by the copy constructors of `BinaryPredicate` or
`ForwardIt`.

## std::default_searcher::operator()

```cpp
template< class ForwardIt2 >
std::pair<ForwardIt2, ForwardIt2>
    operator()( ForwardIt2 first, ForwardIt2 last ) const;  // (since C++17) (until C++20)
template< class ForwardIt2 >
constexpr std::pair<ForwardIt2, ForwardIt2>
    operator()( ForwardIt2 first, ForwardIt2 last ) const;  // (since C++20)
```

The member function called by the Searcher overload of `std::search` to perform
a search with this searcher.

Returns a pair of iterators `i, j`, where `i` is `std::search(first, last,
pat_first, pat_last, pred)` and `j` is `std::next(i, std::distance(pat_first,
pat_last))` unless `std::search` returned `last` (no match), in which case `j`
equals `last` as well.

### Parameters

- **first, last** — a pair of iterators designating the string to be examined

### Return value

A pair of iterators to the first and one past last positions in
`[``first``,``last``)` where a subsequence that compares equal to
`[``pat_first``,``pat_last``)` as defined by `pred` is located, or a pair of
copies of `last` otherwise.

### Example

```cpp
#include <algorithm>
#include <functional>
#include <iomanip>
#include <iostream>
#include <string_view>

int main()
{
    constexpr std::string_view in =
        "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed "
        "do eiusmod tempor incididunt ut labore et dolore magna aliqua";

    const std::string_view needle{"pisci"};

    auto it = std::search(in.begin(), in.end(),
                  std::default_searcher(
                      needle.begin(), needle.end()));
    if (it != in.end())
        std::cout << "The string " << std::quoted(needle) << " found at offset "
                  << it - in.begin() << '\n';
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
- **boyer_moore_searcher (C++17)** — Boyer-Moore search algorithm implementation
  (class template)
- **boyer_moore_horspool_searcher (C++17)** — Boyer-Moore-Horspool search
  algorithm implementation (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/default_searcher*
