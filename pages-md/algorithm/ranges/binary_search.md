# std::ranges::binary_search

```cpp
Call signature
template< std::forward_iterator I, std::sentinel_for<I> S, class T,
          class Proj = std::identity,
          std::indirect_strict_weak_order<
              const T*,
              std::projected<I, Proj>> Comp = ranges::less >
constexpr bool
    binary_search( I first, S last, const T& value, Comp comp = {}, Proj proj = {} );  // (1) (since C++20)
template< ranges::forward_range R, class T, class Proj = std::identity,
          std::indirect_strict_weak_order<
              const T*,
              std::projected<ranges::iterator_t<R>, Proj>> Comp = ranges::less >
constexpr bool
    binary_search( R&& r, const T& value, Comp comp = {}, Proj proj = {} );  // (2) (since C++20)
```

1) Checks if a projected element equivalent to `value` appears within the range
   `[``first``,``last``)`.

2) Same as (1), but uses `r` as the source range, as if using `ranges::begin(r)`
   as `first` and `ranges::end(r)` as `last`.

For `ranges::binary_search` to succeed, the range `[``first``,``last``)` must be
at least partially ordered with respect to `value`, i.e. it must satisfy all of
the following requirements:

- partitioned with respect to `std::invoke(comp, std::invoke(proj, element),
  value)` (that is, all projected elements for which the expression is `true`
  precedes all elements for which the expression is `false`).
- partitioned with respect to `!std::invoke(comp, value, std::invoke(proj,
  element))`.
- for all elements, if `std::invoke(comp, std::invoke(proj, element), value)` is
  `true` then `!std::invoke(comp, value, std::invoke(proj, element))` is also
  `true`.

A fully-sorted range meets these criteria.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **first, last** — the range of elements to examine
- **r** — the range of elements to examine
- **value** — value to compare the elements to
- **comp** — comparison function to apply to the projected elements
- **proj** — projection to apply to the elements

### Return value

`true` if an element equal to `value` is found, `false` otherwise.

### Complexity

The number of comparisons and projections performed is logarithmic in the
distance between `first` and `last` (at most log2(last - first) + O(1)
comparisons and projections). However, for iterator-sentinel pair that does not
model `std::random_access_iterator`, number of iterator increments is linear.

### Possible implementation

```cpp
struct binary_search_fn
{
    template<std::forward_iterator I, std::sentinel_for<I> S, class T,
             class Proj = std::identity,
             std::indirect_strict_weak_order<
                 const T*,
                 std::projected<I, Proj>> Comp = ranges::less>
    constexpr bool
        operator()(I first, S last, const T& value, Comp comp = {}, Proj proj = {}) const
    {
        first = std::lower_bound(first, last, value, comp);
        return (!(first == last) && !(comp(value, *first)));
    }

    template<ranges::forward_range R, class T, class Proj = std::identity,
             std::indirect_strict_weak_order<
                 const T*,
                 std::projected<ranges::iterator_t<R>, Proj>> Comp = ranges::less>
    constexpr bool operator()(R&& r, const T& value, Comp comp = {}, Proj proj = {}) const
    {
        return (*this)(ranges::begin(r), ranges::end(r), value,
                       std::ref(comp), std::ref(proj));
    }
};

inline constexpr binary_search_fn binary_search;
```

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <ranges>

int main()
{
    constexpr static auto haystack = {1, 3, 4, 5, 9};
    static_assert(std::ranges::is_sorted(haystack));

    for (const int needle : std::views::iota(1)
                          | std::views::take(3))
    {
        std::cout << "Searching for " << needle << ": ";
        std::ranges::binary_search(haystack, needle)
            ? std::cout << "found " << needle << '\n'
            : std::cout << "no dice!\n";
    }
}
```

Output:

```text
Searching for 1: found 1
Searching for 2: no dice!
Searching for 3: found 3
```

### See also

- **ranges::equal_range (C++20)** — returns range of elements matching a
  specific key (niebloid)
- **ranges::lower_bound (C++20)** — returns an iterator to the first element
  *not less* than the given value (niebloid)
- **ranges::upper_bound (C++20)** — returns an iterator to the first element
  *greater* than a certain value (niebloid)
- **ranges::containsranges::contains_subrange (C++23)(C++23)** — checks if the
  range contains the given element or subrange (niebloid)
- **binary_search** — determines if an element exists in a partially-ordered
  range (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/binary_search*
