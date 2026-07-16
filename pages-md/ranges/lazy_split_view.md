# std::ranges::views::lazy_split, std::ranges::lazy_split_view

```cpp
template< ranges::input_range V, ranges::forward_range Pattern >
requires ranges::view<V> &&
         ranges::view<Pattern> &&
         std::indirectly_comparable<ranges::iterator_t<V>,
                                    ranges::iterator_t<Pattern>,
                                    ranges::equal_to> &&
         (ranges::forward_range<V> || /*tiny-range*/<Pattern>)
class lazy_split_view
    : public ranges::view_interface<lazy_split_view<V, Pattern>>  // (1) (since C++20)
namespace views {
    inline constexpr /* unspecified */ lazy_split = /* unspecified */;
}  // (2) (since C++20)
Call signature
template< ranges::viewable_range R, class Pattern >
    requires /* see below */
constexpr ranges::view auto lazy_split( R&& r, Pattern&& pattern );  // (since C++20)
template< class Pattern >
constexpr /* range adaptor closure */ lazy_split( Pattern&& pattern );  // (since C++20)
Helper concepts
template< class R >
concept /*tiny-range*/ =
    ranges::sized_range<R> &&
    requires { /* is-statically-constexpr-sized */<R>; } &&
    (std::remove_reference_t<R>::size() <= 1);  // (3) (exposition only*)
```

1) `lazy_split_view` takes a `view` and a delimiter, and splits the `view` into
subranges on the delimiter.

Two major scenarios are supported:

- The view is an `input_range`, the delimiter is a single element (wrapped in a
  `single_view`).
- The view is a `forward_range`, the delimiter is a `view` of elements.

2) A RangeAdaptorObject. The expression `views::lazy_split(e, f)` is
   expression-equivalent to `lazy_split_view(e, f)`.

3) The exposition-only concept `/*tiny-range*/<Pattern>` is satisfied if
   `Pattern` satisfies `sized_range`, `Pattern::size()` is a constant expression
   and suitable as a template non-type argument, and the value of
   `Pattern::size()` is less than or equal to `1`. Notably, `empty_view` and
   `single_view` satisfy this concept.

`lazy_split_view` models the concepts `forward_range` and `input_range` when the
underlying `view` `V` models respective concepts, and models `common_range` when
`V` models both `forward_range` and `common_range`.

The inner range (ranges::range_reference_t<lazy_split_view>) models the concepts
`forward_range` and `input_range` when the underlying `view` `V` models
respective concepts. It does not model `common_range`, and cannot be used with
algorithms that expect a `bidirectional_range` or higher.

### Data members

- **`base_` (private)** — The underlying `view` of type `V`. (exposition-only
  member object*)
- **`pattern_` (private)** — The pattern that is used as a delimiter to split
  the underlying `view`. (exposition-only member object*)
- **`current_` (private) (optional)** — An *optional-like* caching object of
  type /*non-propagating-cache*/<ranges::iterator_t<V>>. Caches the result of
  calls to `begin()`. Present only if `V` does not satisfy `forward_range`.
  (exposition-only member object*)

### Member functions

- **(constructor) (C++20)** — constructs a `lazy_split_view` (public member
  function)
- **base (C++20)** — returns a copy of the underlying (adapted) view (public
  member function)
- **begin (C++20)** — returns an iterator to the beginning (public member
  function)
- **end (C++20)** — returns an iterator or a sentinel to the end (public member
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

### Nested classes

- ***outer_iterator* (C++20)** — the iterator type (exposition-only member class
  template*)
- ***inner_iterator* (C++20)** — the iterator type of the inner range
  (exposition-only member class template*)

### Deduction guides

### Notes

The name `lazy_split_view` is introduced by the post-C++20 defect report
P2210R2. It has the same lazy mechanism as that of the old `split_view` before
change.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <ranges>
#include <string_view>

auto print = [](auto const& view)
{
    // `view` is of std::views::lazy_split_view::__outer_iterator::value_type

    for (std::cout << "{ "; const auto element : view)
        std::cout << element << ' ';
    std::cout << "} ";
};

int main()
{
    constexpr static auto source = {0, 1, 0, 2, 3, 0, 4, 5, 6, 0, 7, 8, 9};
    constexpr int delimiter{0};
    constexpr std::ranges::lazy_split_view outer_view{source, delimiter};
    std::cout << "splits[" << std::ranges::distance(outer_view) << "]:  ";
    for (auto const& inner_view: outer_view)
        print(inner_view);

    constexpr std::string_view hello{"Hello C++ 20 !"};
    std::cout << "\n" "substrings: ";
    std::ranges::for_each(hello | std::views::lazy_split(' '), print);

    constexpr std::string_view text{"Hello-+-C++-+-20-+-!"};
    constexpr std::string_view delim{"-+-"};
    std::cout << "\n" "substrings: ";
    std::ranges::for_each(text | std::views::lazy_split(delim), print);
}
```

Output:

```text
splits[5]:  { } { 1 } { 2 3 } { 4 5 6 } { 7 8 9 }
substrings: { H e l l o } { C + + } { 2 0 } { ! }
substrings: { H e l l o } { C + + } { 2 0 } { ! }
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2210R2 | C++20 | the old `split_view` was too lazy to be easily used | moves
      its functionality to `lazy_split_view`

### See also

- **ranges::split_viewviews::split (C++20)** — a `view` over the subranges
  obtained from splitting another `view` using a delimiter (class template)
  (range adaptor object)
- **ranges::join_viewviews::join (C++20)** — a `view` consisting of the sequence
  obtained from flattening a `view` of `range`s (class template) (range adaptor
  object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/lazy_split_view*
