# std::ranges::views::join_with, std::ranges::join_with_view

```cpp
template< ranges::input_range V, ranges::forward_range Pattern >
    requires ranges::view<V> and
             ranges::input_range<ranges::range_reference_t<V>> and
             ranges::view<Pattern> and
             /* ranges::range_reference_t<V> and Pattern
                have compatible elements (see below) */
class join_with_view
    : ranges::view_interface<join_with_view<V, Pattern>>  // (1) (since C++23)
namespace views {
    inline constexpr /* unspecified */ join_with = /* unspecified */;
}  // (2) (since C++23)
Call signature
template< ranges::viewable_range R, class Pattern >
    requires /* see below */
constexpr ranges::view auto join_with( R&& r, Pattern&& pattern );  // (since C++23)
template< class Pattern >
constexpr /* range adaptor closure */ join_with( Pattern&& pattern );  // (since C++23)
```

1) A range adaptor that represents `view` consisting of the sequence obtained
   from flattening a view of ranges, with every element of the delimiter
   inserted in between elements of the view. The delimiter can be a single
   element or a view of elements.

2) RangeAdaptorObject. The expression `views::join_with(e, f)` is
   expression-equivalent to `join_with_view(e, f)` for any suitable
   subexpressions `e` and `f`.

ranges::range_reference_t<V> and `Pattern` have compatible elements if all of
the following concepts are modeled:

- `std::common_with<ranges::range_value_t<ranges::range_reference_t<V>>,
  ranges::range_value_t<Pattern>>`
-
  `std::common_reference_with<ranges::range_reference_t<ranges::range_reference_t<V>>,
  ranges::range_reference_t<Pattern>>`
-
  `std::common_reference_with<ranges::range_rvalue_reference_t<ranges::range_reference_t<V>>,
  ranges::range_rvalue_reference_t<Pattern>>`

`join_with_view` models `input_range`.

`join_with_view` models `forward_range` when:

- ranges::range_reference_t<V> is a reference, and
- `V` and ranges::range_reference_t<V> each model `forward_range`.

`join_with_view` models `bidirectional_range` when:

- ranges::range_reference_t<V> is a reference,
- `V`, ranges::range_reference_t<V>, and `Pattern` each models
  `bidirectional_range`, and
- ranges::range_reference_t<V> and `Pattern` each model `common_range`.

`join_with_view` models `common_range` when:

- ranges::range_reference_t<V> is a reference, and
- `V` and ranges::range_reference_t<V> each model `forward_range` and
  `common_range`.

### Member functions

- **(constructor) (C++23)** — constructs a `join_with_view` (public member
  function)
- **base (C++23)** — returns a copy of the underlying (adapted) view (public
  member function)
- **begin (C++23)** — returns an iterator to the beginning (public member
  function)
- **end (C++23)** — returns an iterator or a sentinel to the end (public member
  function)

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

### Deduction guides

### Nested classes

- ***iterator* (C++23)** — the iterator type (exposition-only member class
  template*)
- ***sentinel* (C++23)** — the sentinel type (exposition-only member class
  template*)

### Example

```cpp
#include <iostream>
#include <ranges>
#include <string_view>
#include <vector>

int main()
{
    using namespace std::literals;

    std::vector v{"This"sv, "is"sv, "a"sv, "test."sv};
    auto joined = v | std::views::join_with(' ');

    for (auto c : joined)
        std::cout << c;
    std::cout << '\n';
}
```

Output:

```text
This is a test.
```

### See also

- **ranges::join_viewviews::join (C++20)** — a `view` consisting of the sequence
  obtained from flattening a `view` of `range`s (class template) (range adaptor
  object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/join_with_view*
