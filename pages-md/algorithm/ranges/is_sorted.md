# std::ranges::is_sorted

```cpp
Call signature
template< std::forward_iterator I, std::sentinel_for<I> S,
          class Proj = std::identity,
          std::indirect_strict_weak_order<std::projected<I, Proj>>
              Comp = ranges::less >
constexpr bool
    is_sorted( I first, S last, Comp comp = {}, Proj proj = {} );  // (1) (since C++20)
template< ranges::forward_range R, class Proj = std::identity,
          std::indirect_strict_weak_order<
              std::projected<ranges::iterator_t<R>, Proj>>
                  Comp = ranges::less >
constexpr bool
    is_sorted( R&& r, Comp comp = {}, Proj proj = {} );  // (2) (since C++20)
```

Checks if the elements in range `[``first``,``last``)` are sorted in
non-descending order.

A sequence is sorted with respect to a comparator `comp` if for any iterator
`it` pointing to the sequence and any non-negative integer `n` such that `it +
n` is a valid iterator pointing to an element of the sequence,
`std::invoke(comp, std::invoke(proj, *(it + n)), std::invoke(proj, *it))`
evaluates to `false`.

1) Elements are compared using the given binary comparison function `comp`.

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

- **first, last** — iterator-sentinel defining the range to check if it is
  sorted
- **r** — the range to check if it is sorted
- **comp** — comparison function to apply to the projected elements
- **proj** — projection to apply to the elements

### Return value

`true` if the elements in the range are sorted according to `comp`.

### Complexity

Linear in the distance between `first` and `last`.

### Possible implementation

```cpp
struct is_sorted_fn
{
    template<std::forward_iterator I, std::sentinel_for<I> S,
             class Proj = std::identity,
             std::indirect_strict_weak_order<std::projected<I, Proj>>
                 Comp = ranges::less>
    constexpr bool operator()(I first, S last, Comp comp = {}, Proj proj = {}) const
    {
        return ranges::is_sorted_until(first, last, comp, proj) == last;
    }

    template<ranges::forward_range R, class Proj = std::identity,
             std::indirect_strict_weak_order<
                 std::projected<ranges::iterator_t<R>, Proj>>
                     Comp = ranges::less>
    constexpr bool operator()(R&& r, Comp comp = {}, Proj proj = {}) const
    {
        return (*this)(ranges::begin(r), ranges::end(r), std::ref(comp), std::ref(proj));
    }
};

inline constexpr is_sorted_fn is_sorted;
```

### Notes

`ranges::is_sorted` returns `true` for empty ranges and ranges of length one.

### Example

```cpp
#include <algorithm>
#include <array>
#include <functional>
#include <iostream>
#include <iterator>

int main()
{
    namespace ranges = std::ranges;

    std::array digits {3, 1, 4, 1, 5};

    ranges::copy(digits, std::ostream_iterator<int>(std::cout, " "));
    ranges::is_sorted(digits)
        ? std::cout << ": sorted\n"
        : std::cout << ": not sorted\n";

    ranges::sort(digits);

    ranges::copy(digits, std::ostream_iterator<int>(std::cout, " "));
    ranges::is_sorted(ranges::begin(digits), ranges::end(digits))
        ? std::cout << ": sorted\n"
        : std::cout << ": not sorted\n";

    ranges::reverse(digits);

    ranges::copy(digits, std::ostream_iterator<int>(std::cout, " "));
    ranges::is_sorted(digits, ranges::greater {})
        ? std::cout << ": sorted (with 'greater')\n"
        : std::cout << ": not sorted\n";
}
```

Output:

```text
3 1 4 1 5 : not sorted
1 1 3 4 5 : sorted
5 4 3 1 1 : sorted (with 'greater')
```

### See also

- **ranges::is_sorted_until (C++20)** — finds the largest sorted subrange
  (niebloid)
- **is_sorted (C++11)** — checks whether a range is sorted into ascending order
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/is_sorted*
