# std::ranges::minmax_element, std::ranges::minmax_element_result

```cpp
Call signature
template< std::forward_iterator I, std::sentinel_for<I> S, class Proj = std::identity,
          std::indirect_strict_weak_order<std::projected<I, Proj>> Comp = ranges::less >
constexpr minmax_element_result<I>
    minmax_element( I first, S last, Comp comp = {}, Proj proj = {} );  // (1) (since C++20)
template< ranges::forward_range R, class Proj = std::identity,
          std::indirect_strict_weak_order<
              std::projected<ranges::iterator_t<R>, Proj>> Comp = ranges::less >
constexpr minmax_element_result<ranges::borrowed_iterator_t<R>>
    minmax_element( R&& r, Comp comp = {}, Proj proj = {} );  // (2) (since C++20)
Helper types
template< class I >
using minmax_element_result = ranges::min_max_result<I>;  // (3) (since C++20)
```

1) Finds the smallest and largest elements in the range `[``first``,``last``)`.

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

- **first, last** — iterator-sentinel pair denoting the range to examine
- **r** — the range to examine
- **comp** — comparison to apply to the projected elements
- **proj** — projection to apply to the elements.

### Return value

An object consisting of an iterator to the smallest element as the first element
and an iterator to the greatest element as the second. Returns `{first, first}`
if the range is empty. If several elements are equivalent to the smallest
element, the iterator to the first such element is returned. If several elements
are equivalent to the largest element, the iterator to the last such element is
returned.

### Complexity

At most `max(floor((3 / 2) * (N − 1)), 0)` applications of the comparison and
twice as many applications of the projection, where `N = ranges::distance(first,
last)`.

### Possible implementation

```cpp
struct minmax_element_fn
{
  template<std::forward_iterator I, std::sentinel_for<I> S, class Proj = std::identity,
           std::indirect_strict_weak_order<std::projected<I, Proj>> Comp = ranges::less>
  constexpr ranges::minmax_element_result<I>
      operator()(I first, S last, Comp comp = {}, Proj proj = {}) const
  {
    auto min = first, max = first;

    if (first == last || ++first == last) {
      return {min, max};
    }

    if (std::invoke(comp, std::invoke(proj, *first), std::invoke(proj, *min))) {
      min = first;
    } else {
      max = first;
    }

    while (++first != last) {
      auto i = first;
      if (++first == last) {
        if (std::invoke(comp, std::invoke(proj, *i), std::invoke(proj, *min))) {
          min = i;
        }
        else if (!(std::invoke(comp, std::invoke(proj, *i), std::invoke(proj, *max)))) {
          max = i;
        }
        break;
      } else {
        if (std::invoke(comp, std::invoke(proj, *first), std::invoke(proj, *i))) {
          if (std::invoke(comp, std::invoke(proj, *first), std::invoke(proj, *min))) {
            min = first;
          }
          if (!(std::invoke(comp, std::invoke(proj, *i), std::invoke(proj, *max)))) {
            max = i;
          }
        } else {
          if (std::invoke(comp, std::invoke(proj, *i), std::invoke(proj, *min))) {
            min = i;
          }
          if (!(std::invoke(comp, std::invoke(proj, *first), std::invoke(proj, *max)))) {
            max = first;
          }
        }
      }
    }
    return {min, max};
  }

  template<ranges::forward_range R, class Proj = std::identity,
           std::indirect_strict_weak_order<
               std::projected<ranges::iterator_t<R>, Proj>> Comp = ranges::less>
  constexpr ranges::minmax_element_result<ranges::borrowed_iterator_t<R>>
      operator()(R&& r, Comp comp = {}, Proj proj = {}) const
  {
    return (*this)(ranges::begin(r), ranges::end(r), std::ref(comp), std::ref(proj));
  }
};

inline constexpr minmax_element_fn minmax_element;
```

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
namespace rng = std::ranges;

int main()
{
    const auto v = {3, 9, 1, 4, 1, 2, 5, 9};
    const auto [min, max] = rng::minmax_element(v);
    std::cout
        << "min = " << *min << ", at [" << rng::distance(v.begin(), min) << "]\n"
        << "max = " << *max << ", at [" << rng::distance(v.begin(), max) << "]\n";
}
```

Output:

```text
min = 1, at [2]
max = 9, at [7]
```

### See also

- **ranges::min_element (C++20)** — returns the smallest element in a range
  (niebloid)
- **ranges::max_element (C++20)** — returns the largest element in a range
  (niebloid)
- **ranges::minmax (C++20)** — returns the smaller and larger of two elements
  (niebloid)
- **minmax_element (C++11)** — returns the smallest and the largest elements in
  a range (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/minmax_element*
