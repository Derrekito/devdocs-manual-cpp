# std::ranges::search

```cpp
Call signature
template< std::forward_iterator I1, std::sentinel_for<I1> S1,
          std::forward_iterator I2, std::sentinel_for<I2> S2,
          class Pred = ranges::equal_to,
          class Proj1 = std::identity,
          class Proj2 = std::identity >
requires std::indirectly_comparable<I1, I2, Pred, Proj1, Proj2>
constexpr ranges::subrange<I1>
    search( I1 first1, S1 last1, I2 first2, S2 last2, Pred pred = {},
            Proj1 proj1 = {}, Proj2 proj2 = {} );  // (1) (since C++20)
template< ranges::forward_range R1, ranges::forward_range R2,
          class Pred = ranges::equal_to,
          class Proj1 = std::identity,
          class Proj2 = std::identity>
requires std::indirectly_comparable<ranges::iterator_t<R1>,
                                    ranges::iterator_t<R2>, Pred, Proj1, Proj2>
constexpr ranges::borrowed_subrange_t<R1>
    search( R1&& r1, R2&& r2, Pred pred = {}, Proj1 proj1 = {}, Proj2 proj2 = {} );  // (2) (since C++20)
```

1) Searches for the *first* occurrence of the sequence of elements
   `[``first2``,``last2``)` in the range `[``first1``,``last1``)`. Elements are
   compared using binary predicate `pred` after being projected with `proj2` and
   `proj1`, respectively.

2) Same as (1), but uses `r1` as the first source range and `r2` as the second
   source range, as if using `ranges::begin(r1)` as `first1`, `ranges::end(r1)`
   as `last1`, `ranges::begin(r2)` as `first2`, and `ranges::end(r2)` as
   `last2`.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **first1, last1** — the range of elements to examine (aka *haystack*)
- **first2, last2** — the range of elements to search for (aka *needle*)
- **r1** — the range of elements to examine (aka *haystack*)
- **r2** — the range of elements to search for (aka *needle*)
- **pred** — binary predicate to apply to the projected elements
- **proj1** — projection to apply to the elements in the first range
- **proj2** — projection to apply to the elements in the second range

### Return value

1) Returns a `ranges::subrange` value that is the first occurrence of the
   sequence `[``first2``,``last2``)` (aka *needle*) in the range
   `[``first1``,``last1``)` (aka *haystack*), after application of the
   projections `proj1` and `proj2` to the elements of both sequences
   respectively with consequencing application of the binary predicate `pred` to
   compare projected elements. If no such occurrence is found,
   `ranges::subrange{last1, last1}` is returned. If the range to search for (aka
   *needle*) is empty, that is `first2 == last2`, then the
   `ranges::subrange{first1, first1}` is returned.

2) Same as (1) but the return type is `ranges::borrowed_subrange_t<R1>`.

### Complexity

At most `S * N` applications of the corresponding predicate and each projection,
where
(1) `S = ranges::distance(first2, last2)` and `N = ranges::distance(first1,
last1)`;
(2) `S = ranges::distance(r2)` and `N = ranges::distance(r1)`.

### Possible implementation

```cpp
struct search_fn
{
    template<std::forward_iterator I1, std::sentinel_for<I1> S1,
             std::forward_iterator I2, std::sentinel_for<I2> S2,
             class Pred = ranges::equal_to,
             class Proj1 = std::identity,
             class Proj2 = std::identity>
    requires std::indirectly_comparable<I1, I2, Pred, Proj1, Proj2>
    constexpr ranges::subrange<I1>
        operator()(I1 first1, S1 last1, I2 first2, S2 last2, Pred pred = {},
                   Proj1 proj1 = {}, Proj2 proj2 = {}) const
    {
        for (;; ++first1)
        {
            I1 it1 = first1;
            for (I2 it2 = first2;; ++it1, ++it2)
            {
                if (it2 == last2)
                    return {first1, it1};
                if (it1 == last1)
                    return {it1, it1};
                if (!std::invoke(pred, std::invoke(proj1, *it1), std::invoke(proj2, *it2)))
                    break;
            }
        }
    }

    template<ranges::forward_range R1, ranges::forward_range R2,
             class Pred = ranges::equal_to,
             class Proj1 = std::identity,
             class Proj2 = std::identity>
    requires std::indirectly_comparable<ranges::iterator_t<R1>,
                                        ranges::iterator_t<R2>, Pred, Proj1, Proj2>
    constexpr ranges::borrowed_subrange_t<R1>
        operator()(R1&& r1, R2&& r2, Pred pred = {},
                   Proj1 proj1 = {}, Proj2 proj2 = {}) const
    {
        return (*this)(ranges::begin(r1), ranges::end(r1),
                       ranges::begin(r2), ranges::end(r2),
                       std::move(pred), std::move(proj1), std::move(proj2));
    }
};

inline constexpr search_fn search {};
```

### Example

```cpp
#include <algorithm>
#include <cctype>
#include <iostream>
#include <iterator>
#include <string_view>

using namespace std::literals;

void print(int id, const auto& haystack, const auto& needle, const auto& found)
{
    std::cout << id << ") search(\"" << haystack << "\", \"" << needle << "\"); ";
    const auto first = std::distance(haystack.begin(), found.begin());
    const auto last = std::distance(haystack.begin(), found.end());
    if (found.empty())
        std::cout << "not found;";
    else
    {
        std::cout << "found: \"";
        for (const auto x : found)
            std::cout << x;
        std::cout << "\";";
    }
    std::cout << " subrange: {" << first << ", " << last << "}\n";
}

int main()
{
    constexpr auto haystack {"abcd abcd"sv};
    constexpr auto needle {"bcd"sv};

    // the search uses iterator pairs begin()/end():
    constexpr auto found1 = std::ranges::search(
        haystack.begin(), haystack.end(),
        needle.begin(), needle.end());
    print(1, haystack, needle, found1);

    // the search uses ranges r1, r2:
    constexpr auto found2 = std::ranges::search(haystack, needle);
    print(2, haystack, needle, found2);

    // 'needle' range is empty:
    constexpr auto none {""sv};
    constexpr auto found3 = std::ranges::search(haystack, none);
    print(3, haystack, none, found3);

    // 'needle' will not be found:
    constexpr auto awl {"efg"sv};
    constexpr auto found4 = std::ranges::search(haystack, awl);
    print(4, haystack, awl, found4);

    // the search uses custom comparator and projections:
    constexpr auto bodkin {"234"sv};
    auto found5 = std::ranges::search(haystack, bodkin,
        [](const int x, const int y) { return x == y; }, // pred
        [](const int x) { return std::toupper(x); }, // proj1
        [](const int y) { return y + 'A' - '1'; }); // proj2
    print(5, haystack, bodkin, found5);
}
```

Output:

```text
1) search("abcd abcd", "bcd"); found: "bcd"; subrange: {1, 4}
2) search("abcd abcd", "bcd"); found: "bcd"; subrange: {1, 4}
3) search("abcd abcd", ""); not found; subrange: {0, 0}
4) search("abcd abcd", "efg"); not found; subrange: {9, 9}
5) search("abcd abcd", "234"); found: "bcd"; subrange: {1, 4}
```

### See also

- **ranges::adjacent_find (C++20)** — finds the first two adjacent items that
  are equal (or satisfy a given predicate) (niebloid)
- **ranges::findranges::find_ifranges::find_if_not (C++20)(C++20)(C++20)** —
  finds the first element satisfying specific criteria (niebloid)
- **ranges::find_end (C++20)** — finds the last sequence of elements in a
  certain range (niebloid)
- **ranges::find_first_of (C++20)** — searches for any one of a set of elements
  (niebloid)
- **ranges::containsranges::contains_subrange (C++23)(C++23)** — checks if the
  range contains the given element or subrange (niebloid)
- **ranges::includes (C++20)** — returns `true` if one sequence is a subsequence
  of another (niebloid)
- **ranges::mismatch (C++20)** — finds the first position where two ranges
  differ (niebloid)
- **ranges::search_n (C++20)** — searches for a number consecutive copies of an
  element in a range (niebloid)
- **search** — searches for a range of elements (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/search*
