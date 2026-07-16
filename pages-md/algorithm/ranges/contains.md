# std::ranges::contains, std::ranges::contains_subrange

```cpp
Call signature
template< std::input_iterator I, std::sentinel_for<I> S,
          class T, class Proj = std::identity >
requires std::indirect_binary_predicate<ranges::equal_to, std::projected<I, Proj>,
                                        const T*>
constexpr bool contains( I first, S last, const T& value, Proj proj = {} );  // (1) (since C++23)
template< ranges::input_range R, class T, class Proj = std::identity >
requires std::indirect_binary_predicate<ranges::equal_to,
                                        std::projected<ranges::iterator_t<R>, Proj>,
                                        const T*>
constexpr bool contains( R&& r, const T& value, Proj proj = {} );  // (2) (since C++23)
template< std::forward_iterator I1, std::sentinel_for<I1> S1,
          std::forward_iterator I2, std::sentinel_for<I2> S2,
          class Pred = ranges::equal_to,
          class Proj1 = std::identity, class Proj2 = std::identity >
requires std::indirectly_comparable<I1, I2, Pred, Proj1, Proj2>
constexpr bool contains_subrange( I1 first1, S1 last1,
                                  I2 first2, S2 last2,
                                  Pred pred = {},
                                  Proj1 proj1 = {}, Proj2 proj2 = {} );  // (3) (since C++23)
template< ranges::forward_range R1, ranges::forward_range R2,
          class Pred = ranges::equal_to,
          class Proj1 = std::identity, class Proj2 = std::identity >
requires std::indirectly_comparable<ranges::iterator_t<R1>,
                                    ranges::iterator_t<R2>, Pred, Proj1, Proj2>
constexpr bool contains_subrange( R1&& r1, R2&& r2,
                                  Pred pred = {},
                                  Proj1 proj1 = {}, Proj2 proj2 = {} );  // (4) (since C++23)
```

1) Search-based algorithm that checks whether or not a given range contains a
   value with iterator-sentinel pairs.

2) Same as (1) but uses `r` as the source range, as if using `ranges::begin(r)`
   as `first` and `ranges::end(r)` as `last`.

3) Search-based algorithm that checks whether or not a given range is a subrange
   of another range with iterator-sentinel pairs.

4) Same as (3) but uses `r1` as the first source range and `r2` as the second
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

- **first, last** — the range of elements to examine
- **r** — the range of the elements to examine
- **value** — value to compare the elements to
- **pred** — predicate to apply to the projected elements
- **proj** — projection to apply to the elements

### Return value

1,2) : `ranges::find(std::move(first), last, value, proj) != last`

3,4) : `first2 == last2 || !ranges::search(first1, last1, first2, last2, pred,
   proj1, proj2).empty()`

### Complexity

At most `last - first` applications of the predicate and projection.

### Notes

Up until C++20, we've had to write `std::ranges::find(r, value) !=
std::ranges::end(r)` to determine if a single value is inside a range. And to
check if a range contains a subrange of interest, we use `not
std::ranges::search(haystack, needle).empty()`. While this is accurate, it isn't
necessarily convenient, and it hardly expresses intent (especially in the latter
case). Being able to say `std::ranges::contains(r, value)` addresses both of
these points.

`ranges::contains_subrange`, same as `ranges::search`, but as opposed to
`std::search`, provides no access to Searchers (such as Boyer-Moore).

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_ranges_contains` | 202207L | (C++23) | `std::ranges::contains` and
      `ranges::contains_subrange`

### Possible implementation

```cpp
struct __contains_fn
{
    template<std::input_iterator I, std::sentinel_for<I> S,
             class T, class Proj = std::identity>
    requires std::indirect_binary_predicate<ranges::equal_to, std::projected<I, Proj>,
                                            const T*>
    constexpr bool operator()(I first, S last, const T& value, Proj proj = {}) const
    {
        return ranges::find(std::move(first), last, value, proj) != last;
    }

    template<ranges::input_range R, class T, class Proj = std::identity>
    requires std::indirect_binary_predicate<ranges::equal_to,
                                            std::projected<ranges::iterator_t<R>, Proj>,
                                            const T*>
    constexpr bool operator()(R&& r, const T& value, Proj proj = {}) const
    {
        return (*this)(ranges::begin(r), ranges::end(r), std::move(value), proj);
    }
};

inline constexpr __contains_fn contains {};
```

```cpp
struct __contains_subrange_fn
{
    template<std::forward_iterator I1, std::sentinel_for<I1> S1,
             std::forward_iterator I2, std::sentinel_for<I2> S2,
             class Pred = ranges::equal_to,
             class Proj1 = std::identity, class Proj2 = std::identity>
    requires std::indirectly_comparable<I1, I2, Pred, Proj1, Proj2>
    constexpr bool operator()(I1 first1, S1 last1,
                              I2 first2, S2 last2,
                              Pred pred = {},
                              Proj1 proj1 = {}, Proj2 proj2 = {}) const
    {
        return (first2 == last2) ||
               !ranges::search(first1, last1, first2, last2, pred, proj1, proj2).empty();
    }

    template<ranges::forward_range R1, ranges::forward_range R2,
             class Pred = ranges::equal_to,
             class Proj1 = std::identity, class Proj2 = std::identity>
    requires std::indirectly_comparable<ranges::iterator_t<R1>,
                                        ranges::iterator_t<R2>, Pred, Proj1, Proj2>
    constexpr bool operator()(R1&& r1, R2&& r2,
                              Pred pred = {},
                              Proj1 proj1 = {}, Proj2 proj2 = {}) const
    {
        return (*this)(ranges::begin(r1), ranges::end(r1),
                       ranges::begin(r2), ranges::end(r2), std::move(pred),
                       std::move(proj1), std::move(proj2));
    }
};

inline constexpr __contains_subrange_fn contains_subrange {};
```

### Example

```cpp
#include <algorithm>
#include <array>

int main()
{
    constexpr auto haystack = std::array{3, 1, 4, 1, 5};
    constexpr auto needle = std::array{1, 4, 1};
    constexpr auto bodkin = std::array{2, 5, 2};
    auto increment = [](int x) { return ++x; };
    auto decrement = [](int x) { return --x; };

    static_assert(
        std::ranges::contains(haystack, 4) &&
       !std::ranges::contains(haystack, 6) &&
        std::ranges::contains_subrange(haystack, needle) &&
       !std::ranges::contains_subrange(haystack, bodkin) &&
        std::ranges::contains(haystack, 6, increment) &&
       !std::ranges::contains(haystack, 1, increment) &&
        std::ranges::contains_subrange(haystack, bodkin, {}, increment) &&
       !std::ranges::contains_subrange(haystack, bodkin, {}, decrement) &&
        std::ranges::contains_subrange(haystack, bodkin, {}, {}, decrement)
    );
}
```

### See also

- **ranges::findranges::find_ifranges::find_if_not (C++20)(C++20)(C++20)** —
  finds the first element satisfying specific criteria (niebloid)
- **ranges::search (C++20)** — searches for a range of elements (niebloid)
- **ranges::binary_search (C++20)** — determines if an element exists in a
  partially-ordered range (niebloid)
- **ranges::includes (C++20)** — returns `true` if one sequence is a subsequence
  of another (niebloid)
- **ranges::all_ofranges::any_ofranges::none_of (C++20)(C++20)(C++20)** — checks
  if a predicate is `true` for all, any or none of the elements in a range
  (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/contains*
