# std::ranges::views::slide, std::ranges::slide_view

```cpp
template< ranges::forward_range V >
    requires ranges::view<V>
class slide_view
    : public ranges::view_interface<slide_view<V>>  // (1) (since C++23)
namespace views {
    inline constexpr /* unspecified */ slide = /* unspecified */;
}  // (2) (since C++23)
Call signature
template< ranges::viewable_range R >
constexpr ranges::view auto slide( R&& r, ranges::range_difference_t<R> n );  // (since C++23)
template< class DifferenceType >
constexpr /* range adaptor object */ slide( DifferenceType&& n );  // (since C++23)
Helper concepts
template< class V >
concept __slide_caches_nothing =
    ranges::random_access_range<V> && ranges::sized_range<V>;  // (3) (exposition only*)
template< class V >
concept __slide_caches_last =
    !__slide_caches_nothing<V> &&
    ranges::bidirectional_range<V> && ranges::common_range<V>;  // (4) (exposition only*)
template< class V >
concept __slide_caches_first =
    !__slide_caches_nothing<V> && !__slide_caches_last<V>;  // (5) (exposition only*)
```

1) `slide_view` is a range adaptor that takes a `view` and a number `n` and
   produces a view whose *m* ﻿th element (a "window") is a view over the *m* ﻿th
   through *(m + n - 1)* ﻿th elements of the original view.
Let `s` be the size of the original view. Then the size of produced view is:

- `s - n + 1`, if `s >= n`,
- `​0​` otherwise, and the resulting view is empty.

2) The name `views::slide` denotes a RangeAdaptorObject. Given subexpressions
   `e` and `n`, the expression `views::slide(e, n)` is expression-equivalent to
   `slide_view(e, n)`.

If `n` is not greater than `​0​`, the behavior is undefined.

`slide_view` always models `forward_range`, and models `bidirectional_range`,
`random_access_range`, or `sized_range` if adapted `view` type models the
corresponding concept.

### Data members

- **`base_` (private)** — The underlying `view` of type `V`. (exposition-only
  member object*)
- **`n_` (private)** — The "window size" of type ranges::range_difference_t<V>.
  (exposition-only member object*)
- **`cached_begin_` (private) (optional)** — An optional-like object. Present
  only if `V` models the `__slide_caches_first` (4). (exposition-only member
  object*)
- **`cached_end_` (private) (optional)** — An optional-like object. Present only
  if `V` models the `__slide_caches_last` (5). (exposition-only member object*)

### Member functions

- **(constructor) (C++23)** — constructs a `slide_view` (public member function)
- **begin (C++23)** — returns an iterator to the beginning (public member
  function)
- **end (C++23)** — returns an iterator or a sentinel to the end (public member
  function)
- **size (C++23)** — returns the number of elements. Provided only if the
  underlying (adapted) range satisfies `sized_range`. (public member function)

**Inherited from `std::ranges::view_interface`**

- **empty (C++20)** — returns whether the derived view is empty. Provided if it
  satisfies `sized_range` or `forward_range`. (public member function of
  `std::ranges::view_interface<D>`)
- **cbegin (C++23)** — returns a constant iterator to the beginning of the
  range. (public member function of `std::ranges::view_interface<D>`)
- **cend (C++23)** — returns a sentinel for the constant iterator of the range.
  (public member function of `std::ranges::view_interface<D>`)
- **operator bool (C++20)** — returns whether the derived view is not empty.
  Provided if `ranges::empty` is applicable to it. (public member function of
  `std::ranges::view_interface<D>`)
- **front (C++20)** — returns the first element in the derived view. Provided if
  it satisfies `forward_range`. (public member function of
  `std::ranges::view_interface<D>`)
- **back (C++20)** — returns the last element in the derived view. Provided if
  it satisfies `bidirectional_range` and `common_range`. (public member function
  of `std::ranges::view_interface<D>`)
- **operator[] (C++20)** — returns the nth element in the derived view. Provided
  if it satisfies `random_access_range`. (public member function of
  `std::ranges::view_interface<D>`)

### Deduction guides

### Nested classes

- ***iterator* (C++23)** — the iterator type (exposition-only member class
  template*)
- ***sentinel* (C++23)** — the sentinel type used when `slide_view` is not a
  `common_range` (exposition-only member class template*)

### Helper templates

```cpp
template< class V >
inline constexpr bool ranges::enable_borrowed_range<slide_view<V>> =
    ranges::enable_borrowed_range<V>;  // (since C++23)
```

This specialization of `ranges::enable_borrowed_range` makes `slide_view`
satisfy `borrowed_range` when the underlying view satisfies it.

### Notes

There are similarities between `ranges::adjacent_view` and `ranges::slide_view`:

- Both create a "sliding window" of size `N`.
- Both have the same size `S - N + 1`, where `S` is the size of an adapted
  `view` such that `S >= N > 0`.

The differences between these adapters are:

  View adaptor | `value_type` | The window size `N`
  `ranges::adjacent_view` | A `std::tuple` object | A template parameter
  `ranges::slide_view` | A `range` | A runtime parameter

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_ranges_slide` | 202202L | (C++23) | `std::ranges::slide_view`

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <ranges>

auto print_subrange = [](std::ranges::viewable_range auto&& r)
{
    std::cout << '[';
    for (char space[]{0,0}; auto elem : r)
        std::cout << space << elem, *space = ' ';
    std::cout << "] ";
};

int main()
{
    const auto v = {1, 2, 3, 4, 5, 6};

    std::cout << "All sliding windows of width:\n";
    for (const unsigned width : std::views::iota(1U, 1U + v.size()))
    {
        auto const windows = v | std::views::slide(width);
        std::cout << "W = " << width << ": ";
        std::ranges::for_each(windows, print_subrange);
        std::cout << '\n';
    }
}
```

Output:

```text
All sliding windows of width W:
W = 1: [1] [2] [3] [4] [5] [6]
W = 2: [1 2] [2 3] [3 4] [4 5] [5 6]
W = 3: [1 2 3] [2 3 4] [3 4 5] [4 5 6]
W = 4: [1 2 3 4] [2 3 4 5] [3 4 5 6]
W = 5: [1 2 3 4 5] [2 3 4 5 6]
W = 6: [1 2 3 4 5 6]
```

### References

- C++23 standard (ISO/IEC 14882:2023):

### See also

- **ranges::adjacent_viewviews::adjacent (C++23)** — a `view` consisting of
  tuples of references to adjacent elements of the adapted view (class template)
  (range adaptor object)
- **ranges::chunk_viewviews::chunk (C++23)** — a range of `view`s that are
  `N`-sized non-overlapping successive chunks of the elements of another `view`
  (class template) (range adaptor object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/slide_view*
