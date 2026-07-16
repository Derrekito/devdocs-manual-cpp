# std::ranges::search_n

```cpp
Call signature
template< std::forward_iterator I, std::sentinel_for<I> S, class T,
          class Pred = ranges::equal_to, class Proj = std::identity >
requires std::indirectly_comparable<I, const T*, Pred, Proj>
constexpr ranges::subrange<I>
    search_n( I first, S last, std::iter_difference_t<I> count,
              const T& value, Pred pred = {}, Proj proj = {} );  // (1) (since C++20)
template< ranges::forward_range R, class T, class Pred = ranges::equal_to,
          class Proj = std::identity >
requires std::indirectly_comparable<ranges::iterator_t<R>, const T*, Pred, Proj>
constexpr ranges::borrowed_subrange_t<R>
    search_n( R&& r, ranges::range_difference_t<R> count,
              const T& value, Pred pred = {}, Proj proj = {} );  // (2) (since C++20)
```

1) Searches the range `[``first``,``last``)` for the *first* sequence of `count`
   elements whose projected values are each equal to the given `value` according
   to the binary predicate `pred`.

2) Same as (1), but uses `r` as the source range, as if using `ranges::begin(r)`
   as `first` and `ranges::end(r)` as `last`.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **first, last** — the range of elements to examine (aka *haystack*)
- **r** — the range of elements to examine (aka *haystack*)
- **count** — the length of the sequence to search for
- **value** — the value to search for (aka *needle*)
- **pred** — the binary predicate that compares the projected elements with
  `value`
- **proj** — the projection to apply to the elements of the range to examine

### Return value

1) Returns `std::ranges::subrange` object that contains a pair of iterators in
   the range `[``first``,``last``)` that designate the found subsequence. If no
   such subsequence is found, returns `std::ranges::subrange{last, last}`. If
   `count <= 0`, returns `std::ranges::subrange{first, first}`.

2) Same as (1) but the return type is `ranges::borrowed_subrange_t<R>`.

### Complexity

Linear: at most `ranges::distance(first, last)` applications of the predicate
and the projection.

### Notes

An implementation can improve efficiency of the search *in average* if the
iterators model `std::random_access_iterator`.

### Possible implementation

```cpp
struct search_n_fn
{
    template<std::forward_iterator I, std::sentinel_for<I> S, class T,
             class Pred = ranges::equal_to, class Proj = std::identity>
    requires std::indirectly_comparable<I, const T*, Pred, Proj>
    constexpr ranges::subrange<I>
        operator()(I first, S last, std::iter_difference_t<I> count,
                   const T& value, Pred pred = {}, Proj proj = {}) const
    {
        if (count <= 0)
            return {first, first};
        for (; first != last; ++first)
            if (std::invoke(pred, std::invoke(proj, *first), value))
            {
                I start = first;
                std::iter_difference_t<I> n{1};
                for (;;)
                {
                    if (n++ == count)
                        return {start, std::next(first)}; // found
                    if (++first == last)
                        return {first, first}; // not found
                    if (!std::invoke(pred, std::invoke(proj, *first), value))
                        break; // not equ to value
                }
            }
        return {first, first};
    }

    template<ranges::forward_range R, class T, class Pred = ranges::equal_to,
             class Proj = std::identity>
    requires std::indirectly_comparable<ranges::iterator_t<R>, const T*, Pred, Proj>
    constexpr ranges::borrowed_subrange_t<R>
        operator()(R&& r, ranges::range_difference_t<R> count,
                   const T& value, Pred pred = {}, Proj proj = {}) const
    {
        return (*this)(ranges::begin(r), ranges::end(r),
                       std::move(count), value,
                       std::move(pred), std::move(proj));
    }
};

inline constexpr search_n_fn search_n {};
```

### Example

```cpp
#include <algorithm>
#include <iomanip>
#include <iostream>
#include <iterator>
#include <string>

int main()
{
    static constexpr auto nums = {1, 2, 2, 3, 4, 1, 2, 2, 2, 1};
    constexpr int count{3};
    constexpr int value{2};
    typedef int count_t, value_t;

    constexpr auto result1 = std::ranges::search_n(
        nums.begin(), nums.end(), count, value
    );
    static_assert( // found
        result1.size() == count &&
        std::distance(nums.begin(), result1.begin()) == 6 &&
        std::distance(nums.begin(), result1.end()) == 9
    );

    constexpr auto result2 = std::ranges::search_n(nums, count, value);
    static_assert( // found
        result2.size() == count &&
        std::distance(nums.begin(), result2.begin()) == 6 &&
        std::distance(nums.begin(), result2.end()) == 9
    );

    constexpr auto result3 = std::ranges::search_n(nums, count, value_t{5});
    static_assert( // not found
        result3.size() == 0 &&
        result3.begin() == result3.end() &&
        result3.end() == nums.end()
    );

    constexpr auto result4 = std::ranges::search_n(nums, count_t{0}, value_t{1});
    static_assert( // not found
        result4.size() == 0 &&
        result4.begin() == result4.end() &&
        result4.end() == nums.begin()
    );

    constexpr char symbol{'B'};
    auto to_ascii = [](const int z) -> char { return 'A' + z - 1; };
    auto is_equ = [](const char x, const char y) { return x == y; };

    std::cout << "Find a sub-sequence " << std::string(count, symbol) << " in the ";
    std::ranges::transform(nums, std::ostream_iterator<char>(std::cout, ""), to_ascii);
    std::cout << '\n';

    auto result5 = std::ranges::search_n(nums, count, symbol, is_equ, to_ascii);
    if (not result5.empty())
        std::cout << "Found at position "
                  << std::ranges::distance(nums.begin(), result5.begin()) << '\n';
}
```

Output:

```text
Find a sub-sequence BBB in the ABBCDABBBA
Found at position 6
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
- **ranges::includes (C++20)** — returns `true` if one sequence is a subsequence
  of another (niebloid)
- **ranges::mismatch (C++20)** — finds the first position where two ranges
  differ (niebloid)
- **ranges::search (C++20)** — searches for a range of elements (niebloid)
- **search_n** — searches a range for a number of consecutive copies of an
  element (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/search_n*
