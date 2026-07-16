# std::ranges::views::split, std::ranges::split_view

```cpp
template< ranges::forward_range V, ranges::forward_range Pattern >
requires ranges::view<V> &&
         ranges::view<Pattern> &&
         std::indirectly_comparable<ranges::iterator_t<V>,
                                    ranges::iterator_t<Pattern>,
                                    ranges::equal_to>
class split_view
    : public ranges::view_interface<split_view<V, Pattern>>  // (1) (since C++20)
namespace views {
    inline constexpr /* unspecified */ split = /* unspecified */;
}  // (2) (since C++20)
Call signature
template< ranges::viewable_range R, class Pattern >
    requires /* see below */
constexpr ranges::view auto split( R&& r, Pattern&& pattern );  // (since C++20)
template< class Pattern >
constexpr /* range adaptor closure */ split( Pattern&& pattern );  // (since C++20)
```

1) `split_view` takes a `view` and a delimiter, and splits the `view` into
   subranges on the delimiter.

2) RangeAdaptorObject. The expression `views::split(e, p)` is
   expression-equivalent to `split_view(e, p)` for any suitable subexpressions
   `e` and `p`.

`split_view` models the concepts `forward_range`, and `common_range` when the
underlying `view` `V` models respective concepts.

The inner range (ranges::range_reference_t<split_view>) is a
ranges::subrange<ranges::iterator_t<V>>, which models `common_range`, models
`sized_range` when ranges::iterator_t<V> models
`std::sized_sentinel_for<ranges::iterator_t<V>>`, and models `contiguous_range`,
`random_access_range`, `bidirectional_range`, and `forward_range` when `V`
models respective concepts.

### Data members

- **`base_` (private)** — The underlying `view` of type `V`. (exposition-only
  member object*)
- **`pattern_` (private)** — The pattern object that is used as a delimiter to
  split the underlying `view`. (exposition-only member object*)
- **`cached_begin_` (private)** — An *optional-like* object of type
  /*non-propagating-cache*/<ranges::subrange<ranges::iterator_t<V>>> that caches
  the result of the first call to `begin()`. (exposition-only member object*)

### Member functions

- **(constructor) (C++20)** — constructs a `split_view` (public member function)
- **base (C++20)** — returns a copy of the underlying (adapted) view (public
  member function)
- **begin (C++20)** — returns an iterator to the beginning (public member
  function)
- **end (C++20)** — returns an iterator or a sentinel to the end (public member
  function)
- ***find_next* (C++20)** — searches for the next occurrence of the pattern
  (exposition-only member function*)

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

### Nested classes

- ***iterator* (C++20)** — the iterator type (exposition-only member class*)
- ***sentinel* (C++20)** — the sentinel type (exposition-only member class*)

### Deduction guides

### Notes

Before P2210R2, `split_view` used a *lazy* mechanism for splitting, and thus
could not keep the bidirectional, random access, or contiguous properties of the
underlying view, or make the iterator type of the inner range same as that of
the underlying view. Consequently, it is redesigned by P2210R2, and the lazy
mechanism is moved to `lazy_split_view`.

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <ranges>
#include <string_view>

int main()
{
    using std::operator""sv;
    constexpr auto words{"Hello^_^C++^_^20^_^!"sv};
    constexpr auto delim{"^_^"sv};

    for (const auto word : std::views::split(words, delim))
        // with string_view's C++23 range constructor:
        std::cout << std::quoted(std::string_view(word)) << ' ';
    std::cout << '\n';
}
```

Output:

```text
"Hello" "C++" "20" "!"
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2210R2 | C++20 | the old `split_view` was too lazy to be easily used | it is
      redesigned

### See also

- **ranges::lazy_split_viewviews::lazy_split (C++20)** — a `view` over the
  subranges obtained from splitting another `view` using a delimiter (class
  template) (range adaptor object)
- **ranges::join_viewviews::join (C++20)** — a `view` consisting of the sequence
  obtained from flattening a `view` of `range`s (class template) (range adaptor
  object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/split_view*
